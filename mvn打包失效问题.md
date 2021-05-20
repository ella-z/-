# mvn打包失效问题
- 问题背景：使用命令行mvn clean package -DskipTests对项目进行打包时，却报出错误显示Error：npm i 运行失败，原因是no such file or directory, access fsevents。
- 问题解决的方法：查询了一下发现node fsevents报错与node的版本相关，突然想起曾经删除过本地的依赖，再重新在本地中重建依赖，以至于本地的依赖中的node与maven私服上的node产生的依赖版本不一致导致产生问题，删除本地的依赖包之后再从mvn中拉依赖包下来。
### 参考
- [【踩坑之旅】Node.js 与 fsevents](https://juejin.cn/post/6844904152561090573)
