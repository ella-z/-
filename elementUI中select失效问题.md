# elementUI中select失效问题
- 问题背景：从请求中获取到对象值，然后赋值给select监控对象obj.testList，select能够正常展示赋的初始值，但是再向下选的时候，新的选择的数值并不会puhs进obj.testList中，但是change函数被触发了。
- 解决方式：
   - obj.testList赋值格式为数组格式，赋值可以显示，那就可能是select的state层次太深了，数据改变的时候并没有触发render函数，那么导致视图没有更新，所以可以使用$forceUpdate()强制更新视图。
