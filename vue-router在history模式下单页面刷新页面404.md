# vue-router在history模式下单页面刷新页面404
- 问题背景：将vue-router的模式设定为history模式，上线后，在页面进行手动刷新的时候，页面变为404。
- 问题产生的原因：history请求的是真实的路由，上线后如果刷新，就会向服务器请求这个路由，但是后台实际没有，所以就会报错404。
- 解决问题的方法：可以在后台nginx中设置，当404时，将页面重定向到index.html上。
```
🌰：
location / {
  try_files $uri $uri/ /index.html;
}
```
- 参考：
   - [vue router history](https://router.vuejs.org/zh/guide/essentials/history-mode.html)

