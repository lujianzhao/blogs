```js
// 正则替换标准日期格（2015-01-01）式中的-
"2015-05-05".replace(/-/g, "/");
// 一定要满足年月日都要有
Date.parse("2015/05/05");
```