### 高亮插件
[highlight](https://highlightjs.org/)<br>
引入`js`与对应主题的`css`

### json格式高亮
应用：读取`redis`缓存的`json`对象到页面显示
```html
<pre><code id="value"></code></pre>
```
```js
$("#value").html(JSON.stringify(data, undefined, 2));
// 加亮
$("pre code").each(function (i, block) {
    hljs.highlightBlock(block);
});
```

### xml格式高亮
应用：`activiti`工作流的流程定义`xml`文件
```html
<pre><code id="defXml"></code></pre>
```
```js
// 替换<>号
data = data.replace(/</g, "&lt;").replace(/>/g, "&gt;")
$("#defXml").html(data);
// 加亮
$("pre code").each(function (i, block) {
    hljs.highlightBlock(block);
});
```