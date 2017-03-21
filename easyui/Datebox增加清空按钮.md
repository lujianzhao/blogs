### 一、修改`jquery.easyui.min.js` 
在`js`中查询`currentText`，修改如下
```js
currentText : "Today",
        cleanText : "Clear", // 英文Text
        closeText : "Close",
        okText : "Ok",
        buttons : [
                {
                    text : function(_abb) {
                        return $(_abb).datebox("options").currentText;
                    },
                    handler : function(_abc) {
                        var now = new Date();
                        $(_abc).datebox("calendar").calendar(
                                {
                                    year : now.getFullYear(),
                                    month : now.getMonth() + 1,
                                    current : new Date(now.getFullYear(), now
                                            .getMonth(), now.getDate())
                                });
                        _aa9(_abc);
                    }
                }, {
                    text : function(_abd) {
                        return $(_abd).datebox("options").closeText;
                    },
                    handler : function(_abe) {
                        $(this).closest("div.combo-panel").panel("close");
                    }
                }, {// 这里开始，整个对象
                    text : function(_abf) {
                        return $(_abf).datebox("options").cleanText;
                    },
                    handler : function(_abg) {
                         // 清空日期
                        $(_abg).combo("setText", "").combo("setValue", "");
                         // 关闭面板
                        $(this).closest("div.combo-panel").panel("close");
                    }
                } ],
```

### 二、修改`easyui-lang-zh_CN.js`
在`js`中查询`currentText`，修改如下
```js
if ($.fn.datebox){
    $.fn.datebox.defaults.currentText = '今天';
    $.fn.datebox.defaults.closeText = '关闭';
    $.fn.datebox.defaults.okText = '确定';
    $.fn.datebox.defaults.cleanText = '清空'; // 中文Text
    $.fn.datebox.defaults.formatter = function(date){
        var y = date.getFullYear();
        var m = date.getMonth()+1;
        var d = date.getDate();
        return y+'-'+(m<10?('0'+m):m)+'-'+(d<10?('0'+d):d);
    };
    $.fn.datebox.defaults.parser = function(s){
        if (!s) return new Date();
        var ss = s.split('-');
        var y = parseInt(ss[0],10);
        var m = parseInt(ss[1],10);
        var d = parseInt(ss[2],10);
        if (!isNaN(y) && !isNaN(m) && !isNaN(d)){
            return new Date(y,m-1,d);
        } else {
            return new Date();
        }
    };
}
if ($.fn.datetimebox && $.fn.datebox){
    $.extend($.fn.datetimebox.defaults,{
        currentText: $.fn.datebox.defaults.currentText,
        closeText: $.fn.datebox.defaults.closeText,
        okText: $.fn.datebox.defaults.okText,
        cleanText: $.fn.datebox.defaults.cleanText // 注册
    });
}
```

### 效果
* 修改前：

![](https://dn-serical.qbox.me/12.png)

* 修改后：

![](https://dn-serical.qbox.me/13.png)