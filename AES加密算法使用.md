# 理解AES加密解密的使用方法
> 很多人对于AES加密并不是很了解，导致互相之间进行加密解密困难。  
> 本文用简单的方式来介绍AES在使用上需要的知识，而不涉及内部算法。最后给出例子来帮助理解AES加密解密的使用方法。

### AES的麻烦
* 相比于其他加密，AES加密似乎模式很多，包括ECB、CBC等等等等，每个模式又包括IV参数和Padding参数，并且，不同语言对AES加密的库设计有区别。这些导致AES加密在不同人之间联调会很麻烦。
### AES属于块加密
* 不难理解，对越长的字符串进行加密，代价越大，所以通常对明文进行分段，然后对每段明文进行加密，最后再拼成一个字符串。块加密的一个要面临的问题就是如何填满最后一块？所以这就是PADDING的作用，使用各种方式填满最后一块字符串，所以对于解密端，也需要用同样的PADDING来找到最后一块中的真实数据的长度。
### 加密模式
* AES分为几种模式，比如ECB，CBC，CFB等等，这些模式除了ECB由于没有使用IV而不太安全，其他模式差别并没有太明显，大部分的区别在IV和KEY来计算密文的方法略有区别。具体可参考[WIKI](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)的说明。
* 另外，AES分为AES128，AES256等，表示期待秘钥的长度，比如AES256秘钥的长度应该是256/8的32字节，一些语言的库会进行自动截取，让人以为任何长度的秘钥都可以。而这其实是有区别的。
### IV的作用
* IV称为初始向量，不同的IV加密后的字符串是不同的，加密和解密需要相同的IV，既然IV看起来和key一样，却还要多一个IV的目的，对于每个块来说，key是不变的，但是只有第一个块的IV是用户提供的，其他块IV都是自动生成。 
* IV的长度为16字节。超过或者不足，可能实现的库都会进行补齐或截断。但是由于块的长度是16字节，所以一般可以认为需要的IV是16字节。
### PADDING
* AES块加密说过，PADDING是用来填充最后一块使得变成一整块，所以对于加密解密两端需要使用同一的PADDING模式，大部分PADDING模式为PKCS5, PKCS7, NOPADDING。
### 加密解密端
* 所以，在设计AES加密的时候：
    - 对于加密端，应该包括：加密秘钥长度，秘钥，IV值，加密模式，PADDING方式。 
    - 对于解密端，应该包括：解密秘钥长度，秘钥，IV值，解密模式，PADDING方式。
## Nodejs实现
* 这里使用Nodejs的cryptojs库模拟AES加密解密
``` javascript
var crypto = require("crypto");

var algorithm='aes-256-cbc';
var key = new Buffer("aaaabbbbccccddddeeeeffffgggghhhh");
var iv = new Buffer("1234567812345678");
function encrypt(text){
    var cipher=crypto.createCipheriv(algorithm,key,iv);
    cipher.update(text,"utf8");
    return cipher.final("base64");
}
function decrypt(text){
    var cipher=crypto.createDecipheriv(algorithm,key,iv);
    cipher.update(text,"base64");
    return cipher.final("utf8");
}

var text="ni你好hao";
var encoded=encrypt(text)
console.log(encoded);
console.log(decrypt(encoded))
```
* 结果如下
> WfH4hzIc3dc0pjxa9V/RgQ==  
> ni你好hao
* nodejs自带的并不能自动配置padding等参数，演示起来并不方便
* 于是使用另一个框架crypto-js的nodejs库实现和之前完全相同的版本
``` javascript
var CryptoJS = require("crypto-js");
var key ="aaaabbbbccccddddeeeeffffgggghhhh";
var iv = "1234567812345678";

function encrypt(text){
    return CryptoJS.AES.encrypt(text,CryptoJS.enc.Utf8.parse(key),{
        iv:CryptoJS.enc.Utf8.parse(iv),
        mode:CryptoJS.mode.CBC,
        padding:CryptoJS.pad.Pkcs7
    })
}

function decrypt(text){
    var result = CryptoJS.AES.decrypt(text,CryptoJS.enc.Utf8.parse(key),{
        iv:CryptoJS.enc.Utf8.parse(iv),
        mode:CryptoJS.mode.CBC,
        padding:CryptoJS.pad.Pkcs7
    })
    return result.toString(CryptoJS.enc.Utf8)
}

var text="ni你好hao";
var encoded=encrypt(text)
console.log(encoded.toString());
console.log(decrypt(encoded))
```
* 现在aes的参数都变成可配置的，接下来验证一下之前对AES的理解。
  * 改变IV的长度，发现当IV大于16字节的时候，不管16字节之后的是什么，都不影响加密结果，应该是种自动截取机制(nodejs原生库IV不是16字节，就会报错）
  * 改变IV的长度，当IV小于16字节，还可以成功加密，可能是自动补齐机制
  * 加密IV和解密IV不同的时候，并不影响解密是否成功，但是解密的结果有差别，比如将解密的IV变成1234567813345678，则解密结果变为ni你好h\`o
  * 修改padding，加密解密的padding换成NoPadding，发现解密之后生成utf8字符串出错
  * 经过多次尝试，加密为Pkcs7和ZeroPadding时，加密后的字符串变化显著，这时解密用任何padding模式，都可以成功解密。
* ni你好hao，经过Pkcs7后，输出为
> WfH4hzIc3dc0pjxa9V/RgQ==
* nopadding后，输出为
> OtSNypfx1SF6C2E=
* zeropadding后，输出为
> OtSNypfx1SF6C2GfyXMidA==
* Pkcs7的结果和其他结果相差很大，很难相信其padding是补充最后一块
* 有趣的是Pkcs7的结果和zeropadding的结果通过同样的解密设置，能解出同样的字符串ni你好hao
## 总结
1. AES加密解密的秘钥有一对，一个是IV一个是KEY，并且他们的长度都有严格要求。 
2. Padding的作用似乎不只是补齐最后，如果自己什么都对，但是加密失败，可以尝试不同Padding。


