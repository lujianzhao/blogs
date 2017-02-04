我们在使用`datagrid`列属性`formatter`时，有些场景下，在`formatter`函数内部，我们需要知道当前列的一些配置信息，比如说`title`,`filed` 等信息，
而`formatter`函数入参只有行索引，值，行数据，那么怎么获取`title`,`filed`等信息呢？其实`formatter`函数内部的`this`在运行时默认就是指向当前列的配置对象，
且组件内部并没有改变`this`指向，所以通过`this`就可以拿到所有信息了。
```js
$('#dg').datagrid({
    columns: [
        [
            {
                field: 'userId',
                title: 'User',
                width: 80,
                formatter: function(value,
                row,
                index){
                    console.log(this.title);//Userconsole.log(this.field);//userIdif(row.user){
                        return row.user.name;
                    }else{
                        return value;
                    }
                }
            }
        ]
    ]
});
```
类似的列属性还有`styler`和`sorter`