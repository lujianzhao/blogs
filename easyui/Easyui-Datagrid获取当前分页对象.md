```js
// 取得当前分页对象
var page = $("#dg").datagrid("getPager").data("pagination").options;
param.page = page.pageNumber;
param.rows = page.pageSize;
```