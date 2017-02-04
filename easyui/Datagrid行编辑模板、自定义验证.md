###一、datagrid行编辑模板
```js
 $(function () {
            initDatagrid(getQueryParams());
        });

        /**获取查询参数*/
        function getQueryParams() {
            var o = {};

            return o;
        }

        /**初始化datagrid*/
        function initDatagrid(param) {
            $("#dg").datagrid({
                idField : "ID",
                title : "干系人列表",
                url : "<%=path%>/xxbs/projectManage/getProjectUserList.page",
                queryParams : param,
                striped : true,
                fit : true,
                fitColumns : true,
                rownumbers : true,
                singleSelect : true,
                loadMsg : "数据加载中，请耐心等待...",
                pageList : [ 5, 10, 20, 30, 50 ],
                pagination : true,
                onDblClickRow : doubleClickRow,
                //onClickRow : clickRow,
                onAfterEdit : afterEdit,
                columns: [[{
                    title : "姓名",
                    field : "NAME",
                    align : "center",
                    width : 100,
                    editor : {
                        type : "validatebox",
                        options : {
                            required : true
                        }
                    }
                },{
                    title : "电话",
                    field : "PHONE",
                    align : "center",
                    width : 100,
                    editor : {
                        type : "validatebox",
                        options : {
                            validType : "phoneOrMobile",
                            required : true
                        }
                    }
                }, {
                    title : "角色",
                    field : "ROLE",
                    align : "center",
                    width : 100,
                    editor : {
                        type : "validatebox",
                        options : {
                            required : true
                        }
                    }
                }, {
                    title : "描述",
                    field : "DESCRIPTION",
                    align : "center",
                    width : 300,
                    editor : {
                        type : "validatebox",
                        options : {
                            required : true
                        }
                    }
                }]],
                toolbar : [{
                    text : "添加",
                    iconCls : "icon-add",
                    handler : function () {
                        append();
                    }
                }, {
                    text : "删除",
                    iconCls : "icon-remove",
                    handler : function () {
                        var row = $("#dg").datagrid("getSelected");
                        if(null == row.ID) {
                            $.messager.alert("系统提示", "请选择一条记录！", "warning");
                            return;
                        }
                        $.messager.confirm("系统提示", "是否确认删除？", function (r) {
                            if(r) {
                                $.post("<%=path%>/xxbs/projectManage/deleteProjectUser.page", 
                                        {ID : row.ID}, function (data) {
                                    doCallback(data);
                                }, "JSON");
                            }
                        });
                    }
                }, {
                    text : "保存",
                    iconCls : "icon-save",
                    handler : function () {
                        clickRow();
                    }
                }, {
                    text : "取消",
                    iconCls : "icon-cancel",
                    handler : function () {
                        reject();
                    }
                }]
            });
        }

        /**处理状态*/
        function doCallback(data) {
            if(typeof data == "string") {
                data = $.parseJSON(data);
            }
            if(data.status == "1") {
                $.messager.alert("系统提示", data.msg, "info");
                $("#dg").datagrid("reload");
            } else if(data.status == "0") {
                $.messager.alert("系统提示", data.msg, "error");
            }
        } 

        /**行编辑函数*/
        var editIndex = undefined;
        function endEditing (){
            if (editIndex == undefined){return true;};
            if ($("#dg").datagrid("validateRow", editIndex)){
                $("#dg").datagrid("endEdit", editIndex);
                editIndex = undefined;
                return true;
            } else {
                return false;
            }
        };

        /**表格双击事件  */
        function doubleClickRow (index, row){
            if (editIndex != index){
                if (endEditing()){
                    $("#dg").datagrid("selectRow", index)
                            .datagrid("beginEdit", index);
                    editIndex = index;
                } else {
                    $("#dg").datagrid("selectRow", editIndex);
                }
            }
        };

        /**取消更改  */
        function reject(){
            // 重要判断
            if(editIndex != undefined) {
                $("#dg").datagrid("rejectChanges");
                editIndex = undefined;
            }
        }

        /**表格单机事件  */
        function clickRow (index, row) {
            if(endEditing()) {
                $("#dg").datagrid("acceptChanges");
            } else {
                $("#dg").datagrid("selectRow", editIndex);
                //$("#dg").datagrid("rejectChanges");
                //editIndex = undefined;
            }
        };

        /**编辑结束事件  */
        function afterEdit (rowIndex, rowData, changes) {
            rowData.PROJECT_ID = parent.getProjectId();
            $.post("<%=path%>/xxbs/projectManage/saveProjectUser.page", 
                    rowData, function (data) {
                if(data.status == "1") {
                    $.messager.alert("系统提示", data.msg, "info");
                    $("#dg").datagrid("reload");
                } else if(data.status == "0") {
                    $.messager.alert("系统提示", data.msg, "error");
                }
            }, "json");
        };

        /**添加事件  */
        function append(){
            if (endEditing()){
                $("#dg").datagrid("appendRow", {
                    NAME : "",
                    PHONE : "",
                    ROLE : "",
                    DESCRIPTION : ""});
                editIndex = $("#dg").datagrid("getRows").length-1;
                $("#dg").datagrid("selectRow", editIndex)
                        .datagrid("beginEdit", editIndex);
            }
        }
```

###二、treegrid行编辑
```js
$(function() {

            inittreegrid(getQueryParams());
        });

        /**重置事件*/
        function resetEvent() {
            $("#CLASSIFY_NAME").val("");
            //$("#CLASSIFY_NO").val("");

            searchEvent();
        }

        /**查询事件*/
        function searchEvent() {
            cancel();
            $("#dg").treegrid("load", getQueryParams());
        }

        /**获取查询参数*/
        function getQueryParams() {
            var o = {};
            o.CLASSIFY_NAME = $("#CLASSIFY_NAME").val();
            //o.CLASSIFY_NO = $("#CLASSIFY_NO").val();

            return o;
        }

        /**初始化treegrid*/
        function inittreegrid(param) {
            $("#dg").treegrid({
                idField : "ID",
                treeField : "CLASSIFY_NAME",
                title : "项目类别列表",
                url : "<%=path%>/xxbs/classifyManage/getClassifyList.page",
                queryParams : param,
                striped : true,
                fit : true,
                lines : true,
                //fitColumns : true,
                rownumbers : true,
                singleSelect : true,
                loadMsg : "数据加载中，请耐心等待...",
                //pageList : [ 5, 10, 20, 30, 50 ],
                //pagination : true,
                onDblClickRow : doubleClickRow,
                onAfterEdit : afterEdit,
                columns: [[{
                    title : "项目名称",
                    field : "CLASSIFY_NAME",
                    align : "left",
                    halign : "center",
                    width : 400,
                    editor : {
                        type : "validatebox",
                        options : {
                            required : true
                        }
                    }
                }]],
                toolbar : [{
                    text : "添加",
                    iconCls : "icon-add",
                    handler : function () {
                        append();
                    }
                }, {
                    text : "保存",
                    iconCls : "icon-save",
                    handler : function () {
                        clickRow(null);
                    }
                }, {
                    text : "取消",
                    iconCls : "icon-cancel",
                    handler : function () {
                        cancel();
                    }
                }]
            });
        }

        /**行编辑函数*/
        var editIndex = undefined;
        function endEditing (){
            if (editIndex == undefined){return true;};
            if ($("#dg").treegrid("validateRow", editIndex)){
                $("#dg").treegrid("endEdit", editIndex);
                editIndex = undefined;
                return true;
            } else {
                return false;
            }
        };

        /**添加行事件*/
        function append() {
            if (endEditing()){
                var node = $("#dg").treegrid("getSelected");
                var pid = null == node ? "" : node.ID;
                editIndex = -1;
                $("#dg").treegrid("append", {
                    parent : pid,
                    data : [{
                        ID : editIndex,
                        PID : pid,
                        CLASSIFY_NAME : "",
                        CLASSIFY_NO : ""
                    }]
                });
                $("#dg").treegrid("beginEdit", editIndex);
            }
        }

        /**行双击事件  */
        function doubleClickRow (row){
            var index = row.ID
            if (editIndex != index){
                if (endEditing()){
                    $("#dg").treegrid("selectRow", index)
                            .treegrid("beginEdit", index);
                    var row = $("#dg").treegrid("getSelected");
                    editIndex = index;
                } else {
                    $("#dg").treegrid("selectRow", editIndex);
                }
            }
        };

        /**行单击事件  */
        function clickRow (row) {
            if(endEditing()) {
                $("#dg").treegrid("acceptChanges");
            } else {
                // 验证不通过直接取消编辑
                $("#dg").treegrid("cancelEdit", editIndex);
                if(editIndex == -1) {
                    $("#dg").treegrid("remove", editIndex);
                }
                editIndex = undefined;
            }
        };

        /**编辑结束事件  */
        function afterEdit (row, changes) {
            row.CLASSIFY_NO = "";
            $.post("<%=path%>/xxbs/classifyManage/saveClassify.page", 
                    row, function (data) {
                if(data.status == "1") {
                    $.messager.alert("系统提示", data.msg, "info");
                    $("#dg").treegrid("reload");
                } else if(data.status == "0") {
                    $.messager.alert("系统提示", data.msg, "error");
                }
                editIndex = undefined;
            }, "json");
        };

        /**取消事件*/
        function cancel(){
            if (editIndex != undefined){
                $("#dg").treegrid("cancelEdit", editIndex);
                if(editIndex == -1) {
                    $("#dg").treegrid("remove", editIndex);
                }
                editIndex = undefined;
            }
        }
```

###三、自定义验证 
引入如下js，在`validatebox`中使用，如`validType : “phoneOrMobile”`,
```js
$.extend($.fn.validatebox.defaults.rules, {
    // 验证电话号码
    phone : {
        validator : function(value) {
            return /^(\d{3,4})(\-?)(\d{7,8})$/i.test(value);
        },
        message : '格式不正确,请使用下面格式:020-88888888'
    },
    // 验证手机号码
    mobile : {
        validator : function(value) {
            return /^(1)\d{10}$/i.test(value);
        },
        message : '手机号码格式不正确(正确格式如：13450774432)'
    },
    // 验证手机或电话
    phoneOrMobile : {
        validator : function(value) {
            if(value.indexOf("-") > 0) {
                return /^(\d{3,4})(\-?)(\d{7,8})$/i.test(value);
            } else {
                return /^(1)\d{10}$/i.test(value)
            }
        },
        message : '请填入手机或电话号码,如13688888888或020-8888888'
    }
});
```