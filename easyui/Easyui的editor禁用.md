```js
$("#dg").datagrid("selectRow", index).datagrid("beginEdit", index);
editIndex = index;
var editor = $("#dg").datagrid("getEditor", {index : editIndex, field : "status"});
// 普通的用porp
//$(editor.target).prop("disabled", true);
// easyui组建用easyui禁用
$(editor.target).combobox("disable");
console.info(editor.target);
其他说明
EasyUI常用控件禁用方法：

1.validatebox可以用的用法:前两种适用于单个的validatebox;
第三种应用于整个form里面的输入框;

 <1>.$("#id").attr("readonly", true);
     $("#id").removeAttr("readonly");

 <2>.$("#id").attr("readonly", "readonly");
     $("#id").removeAttr("readonly");

 <3>.$("#Form :input").attr("readonly", "readonly"); //对form里面的禁用

 <4>.$("input").attr("readonly", "readonly"); //对所有的input标签禁用
2.combobox禁用启用用法:
 <1>.$("#id").combobox({ disabled: true });
     $("#id").combobox({ disabled: false});

 <2>.$("#id").attr("readonly", "readonly");  //对单个禁用
     $("#id").removeAttr("readonly");

 <3>.$("#fm .easyui-combobox").combobox({ disabled: true });  //对form里面的下拉框禁用

 <4>.$("#ID").combobox("disable");
     $("#ID").combobox("enable");
3.datebox与datetimebox禁用启用方法:
 <1>.$("#fm .easyui-datebox").datebox({ disabled: true });
     $("#fm .easyui-datebox").datebox({ disabled: false});

 <2>.$("#id").attr("readonly", "readonly");
     $("#id").removeAttr("readonly");

 <3>.$("#fm .easyui-datetimebox").datetimebox({ disabled: true });
     $("#fm .easyui-datetimebox").datetimebox({ disabled: true });
4.combogrid禁用启用方法:
 <1>.$("#FPayApplySupAccountID").combogrid("disable");
     $("#FPayApplySupAccountID").combogrid("enable");
5.lable标签ID附加文字：
 <1>.$("#id").text("标题:"); //此方法可以屏蔽掉label标签内的文字
 ```