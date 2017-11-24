# 快速重现被打包的javascrpt代码
#### 碰到像下面这样的代码不知道它在做什么？完全读不懂？
``` javascript
 eval(function(p,a,c,k,e,r){e=function(c){return c.toString(a)};if(!''.replace(/^/,String)){while(c--)r[e(c)]=k[c]||e(c);k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('n=c;1=4();i=3;e=0;5(i--){6.7({8:"9",b:"/2/d.f"+"?"+1,g:"h",j:k,l:m(a){o(a);e++;p(e>q){r.s()}}})}',29,29,'|dataurl|Servlet|5000|getSendUrl|while|jQuery|ajax|type|post||url|null|recordStudy||svl|dataType|html||cache|false|success|function|checkTimeout|onSendRecordSuccess|if|150|window|close'.split('|'),0,{}))}
```
~~对，没错，说的就是你！那个锦程网快速刷视频的代码！~~  
嗯······  
emmmmmm  
其实······  
把`eval`去掉然后复制到chrome的console里就好了······  
就成了这样：  
``` javascript
 "checkTimeout=null;dataurl=getSendUrl();i=5000;e=0;while(i--){jQuery.ajax({type:"post",url:"/Servlet/recordStudy.svl"+"?"+dataurl,dataType:"html",cache:false,success:function(a){onSendRecordSuccess(a);e++;if(e>150){window.close()}}})}"
```
去格式化下就好啦！
``` javascript
checkTimeout = null;
dataurl = getSendUrl();
i = 5000;
e = 0;
while (i--) {
    jQuery.ajax({
        type: "post",
        url: "/Servlet/recordStudy.svl" + "?" + dataurl,
        dataType: "html",
        cache: false,
        success: function (a) {
            onSendRecordSuccess(a);
            e++;
            if (e > 150) {
                window.close()
            }
        }
    })
}
```
