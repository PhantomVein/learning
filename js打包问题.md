# �������ֱ����javascrpt�Ĵ���
#### ���������������Ĵ��벻֪��������ʲô����ȫ��������
``` javascript
 eval(function(p,a,c,k,e,r){e=function(c){return c.toString(a)};if(!''.replace(/^/,String)){while(c--)r[e(c)]=k[c]||e(c);k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('n=c;1=4();i=3;e=0;5(i--){6.7({8:"9",b:"/2/d.f"+"?"+1,g:"h",j:k,l:m(a){o(a);e++;p(e>q){r.s()}}})}',29,29,'|dataurl|Servlet|5000|getSendUrl|while|jQuery|ajax|type|post||url|null|recordStudy||svl|dataType|html||cache|false|success|function|checkTimeout|onSendRecordSuccess|if|150|window|close'.split('|'),0,{}))}
```
~~�ԣ�û��˵�ľ����㣡�Ǹ�����������ˢ��Ƶ�Ĵ��룡~~
�š�����������
emmmmmm
��ʵ������������
��`eval`ȥ��Ȼ���Ƶ�==chrome==��==console==��ͺ��ˡ�����������
�ͳ���������
``` javascript
 "checkTimeout=null;dataurl=getSendUrl();i=5000;e=0;while(i--){jQuery.ajax({type:"post",url:"/Servlet/recordStudy.svl"+"?"+dataurl,dataType:"html",cache:false,success:function(a){onSendRecordSuccess(a);e++;if(e>150){window.close()}}})}"
```
ȥ��ʽ���¾ͺ�����
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