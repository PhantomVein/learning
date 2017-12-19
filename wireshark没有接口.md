# wireshark找不到接口的处理方法
* 问题：wireshark提示没有一个可以抓包的接口
* 解决方案：以管理员的身份在cmd中输入命令 net start npf 
-------
win7安装wireshark提示没有一个可以抓包的接口

* 这是由于win10 默认NPF服务是关闭的，需要以管理员的身份开启这个服务
* Windows 10上安装wireshark时，会遇到NPF驱动无法启动的情况，一般如果采用管理员的方式就可以正常启动，或者是将NPF安装为service的方式，这样问题就OK了
* 以管理员的方式进行启动NPF驱动方法步骤为
> 开始->附件->cmd(右键点击，浏览到以管理员方式启动) ->命令net start npf
* 比较快的方式是:
> 按下windows徽标键->键盘输入cmd(此时注意按下徽标键后窗口下的搜索栏内会输入你键入的cmd)，搜到cmd,->同时按下ctrl+shift+enter—>接下来的就是弹出的是否以管理员运行，一切OK了
