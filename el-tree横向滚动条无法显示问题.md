# el-tree横向滚动条无法显示问题
### 问题描述
- 在进行开发的时候发现，el-tree的节点超过了规定的宽度，但是横向滚动条并没有出现。
### 问题的解决
- 将el-tree-node从block改为inline-block
   ```
      /deep/ .el-tree > .el-tree-node {
        display: inline-block;
        min-width: 100%;
      }
   ```
### 参考
- [The [Bug Report] tree component has no width, resulting in no horizontal scroll bar](https://github.com/ElemeFE/element/issues/4944)
