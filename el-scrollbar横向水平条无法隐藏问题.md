# el-scrollbar横向水平条无法隐藏问题
- 当el-scrollbar里面的内容超过高度和宽度，出现横向滚动条和竖向滚动条，原生竖向滚动条能成功被隐藏掉，而横向滚动条无法被隐藏，发现margin-bottom失效。
- 问题的解决方法：原生水平的滚动条无法通过margin-bottom为负值来把它挤出可视区，因为无论div.el-scrollbar__wrap是否设置了高度，margin-bottom都不会影响自身的高度和定位，只有通过将div.el-scrollbar__wrap的高度设置来略大于div.el-scrollbar，才能隐藏掉水平滚动条。
   ```
     /deep/ .el-scrollbar__wrap {
                height: calc(100% + 18px);
                overflow: scroll;
                overflow-x: auto;
              }
   ```

### 参考
- [element-ui的隐藏组件el-scrollbar的使用](https://blog.csdn.net/chenjie9230/article/details/109189299)
