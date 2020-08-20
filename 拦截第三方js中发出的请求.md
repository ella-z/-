# 拦截第三方js中发出的请求
- 问题背景：需要拦截引入第三方js中发出的请求，并获取这个请求的参数，然后将参数合并到新接口中，然后发送请求。
- 解决的过程：
   1. 使用Ajax-hook对请求进行拦截,但是Ajax-hook只能拦截全局的ajax，无法拦截第三方js中的请求。
   ```
   🌰：
   proxy({
    //请求发起前进入
    onRequest: (config, handler) => {
        console.log(config.url);
        if(config.url === 'https://xxxx')
        {
          .......
        }
        handler.next(config);
    },
    //请求发生错误时进入，比如超时；注意，不包括http状态码错误，如404仍然会认为请求成功
    onError: (err, handler) => {
        console.log(err.type)
        handler.next(err)
    },
    //请求成功后进入
    onResponse: (response, handler) => {
        console.log(response.response)
        handler.next(response)
    }
  })
   ```
   2. 使用service-worker，service-worker可以拦截所有的网络请求，可以在sw.js中设置addEventListener监控fetch，然后根据拦截到的请求进行请求转发,成功实现了需求。
   ```
   🌰：
   //sw.js中
   sw.addEventListener('fetch', (event) => {
    if(event.requset.url === 'https://xxxx'){
        .......
    }
   })
   ```
- 参考链接：
   - [Ajax-hook](https://github.com/wendux/Ajax-hook)
   - [Service Worker](https://www.jianshu.com/p/768be2733872)

