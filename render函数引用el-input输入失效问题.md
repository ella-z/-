# render函数引用el-input输入失效问题
#### 问题的产生
- 因需求，需要实现el-tree的节点点击重命名功能。在重命名时，需要将span改为el-input，但是el-input中并不能正常显示变量中已有的值而且无法输入新的值。
   ```
      h("el-input", {
          style: "width:85%",
          attrs: {
            size: "mini",
          },
          domProps: {
            value: this.nameValue,
          },
          on: {
            input: (val) => {
              this.nameValue = val;
            },
          },
        })
   ```
#### 解决问题
   - 将domProps改为props
   ```
     h("el-input", {
          style: "width:85%",
          attrs: {
            size: "mini",
          },
          props: {
            value: _this.nameValue,
          },
          on: {
            input: (val) => {
              _this.nameValue = val;
            },
          },
        });
   ```
   
#### 参考
- [手把手教你玩转render函数](https://juejin.cn/post/6969226302767235108)
