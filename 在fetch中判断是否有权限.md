# 在fetch中判断是否有权限
- 问题背景：因为鉴权在后端实现，所以当用户请求没有访问权限的接口数据的时候，会返回auth_fail。需求：当接口返回的值为auth_fail时，弹出提示弹框。若为正常的json数据返回时，就直接返回数据。
- 解决问题的思路：
   1. 因为所有的请求都是通过fetch进行请求的，而fetch是基于promise API设计的。先来看看普通的fetch请求👇。
   ```
     🌰：
     fetch('https:xxxxx')
    .then(response => {
      return response.json(); //因为现在的response数据是可读数据流，所以需要将数据转换为可读的json类型数据
    })
    .then(  res=> {
      console.log(res); //输出数据
    });
   ```
   2. 若像上面的代码一样，当返回的是auth_fail时，因为不是符合json类型的数据流，所以response.json()无法正常转换会抛出错误。而在response时期无法识别数据流的里面的数据内容，但又想到response的还有
   一个text()方法，可以将可读数据流转化为字符串类型的可读数据。那可不可以将可读数据流中的数据转化为可读的字符串类型数据再进行判断，若为auth_fail，则弹出提示框。若不为auth_fail，就将该数据转换为
   json类型的数据并返回。
   改造~👇：
   ```
   🌰：
     fetch('https:xxxxx')
    .then(response => {
      const result = response.text().then(res => {
           return res;
      })
      return result; 
    })
    .then(res=>{
      if(res === 'auth_fail')
      {
        alert('auth_fail');
        return;
      }else{
        return JSON.parse(res);
      }
    })
    .then(  res=> {
      console.log(res); //输出数据
    });
   ```
   3. 完成~撒花~❀
- 解决问题中遇到的坑：
   - 第一版的想法：先将response转化为text()类型的数据，然后再将判断转换后的值，若不为auth_fail的时候，用json()方法将response转化为json类型的数据。
      - 结果：Failed to execute 'json' on 'Response': body stream is locked 
      - 原因： response.json 和 response.text 方法只能使用一个并且只能使用一次，同时使用两个或则使用两次都会报错。因为数据流只能读取一次，一旦读取，数据流变空，再次读取会报错。
      








   
   
