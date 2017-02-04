###取当前页page，rows
```js
var page = $("#dg").datagrid("getPager").data("pagination").options;
param.page = page.pageNumber;
param.rows = page.pageSize;
// 第二种
$("#dg").datagrid().data("datagrid").options.pageSize
$("#dg").datagrid().data("datagrid").options.pageNumber
```

###easyui遮罩层
```js
/**处理等待  */
function onLoading(divId) {
    var offset = $(divId).offset();

    $("<div class='datagrid-mask'></div>").css({
        display : "block",
        width : $(divId).width(),
        height : $(divId).height(),
        left: offset.left,
        top: offset.top
    }).appendTo(divId);
    $("<div class='datagrid-mask-msg'></div>").html("正在处理中，不要重复点击导出，请稍候...")
        .appendTo(divId).css({
            display : "block",
            left : ($(document.body).outerWidth(true) - 190) / 2,
            top : offset.top + ($(divId).height() - 45) / 2
        });
}

/**移除等待  */
function removeLoad(divId) {
    //console.info($(divId+" .datagrid-mask"));
    $(divId+" .datagrid-mask").remove();
    $(divId+" .datagrid-mask-msg").remove();
    //$(divId+" .time-item").css("visibility", "visible");
    $(divId).css("visibility", "visible");
}
```

###考试右上角时间一直跟随滚动
```js
/**滚动事件  */
function scrollEvent() {
    var top = $(".time-item").offset().top + 28;
    // 设置倒计时left
    var width = $(window).width() -320 + "px";
    // 动态设置绝对位置,整个div才可见
    $(".time-item").css({"position":"absolute","top":"28px", "left":width});  
    $(window).scroll(function () {
        var offsetTop = top + $(window).scrollTop() + "px";
        // 动画效果
        $(".time-item").animate({top : offsetTop },{duration:600, queue:false});  
        // 无动画效果
        //$(".time-item").css({"position":"absolute","left":width,"top":offsetTop});
    });
}
jquery data()
<div id="t1" data-test="test111"></div>
// 打印test111
console.info($("#t1").data("test"));           
console.info($.data($("#t1")[0], "test"));

$("#t1").data("a", "b");           
// 打印b
console.info($("#t1").data("a"));
```