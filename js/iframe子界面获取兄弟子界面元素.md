```js
// 设置父界面input文件id
var input = $(parent.document).find("#" + input_id);
if(input.length > 0) {
    $(input).val(rs.id);
// 父界面不存在该元素则循环查询所有兄弟iframe
} else {
     parent.$("iframe").each(function(i, o){
        input = $(o).contents().find("#" + input_id);
        if(input.length > 0) {
            $(input).val(rs.id);
        }
     });
}
```