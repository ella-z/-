# 使用render函数渲染el-input的问题
### el-input无法正常输入值
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

### el-input无法自动获取焦点
#### 问题的产生
- 因需求，需要实现渲染el-input后，el-input立即获取得到焦点。
#### 解决问题的方法
##### 1.尝试使用autofocus
- 失败，因为在element中el-input组件外面还有其他组件，导致autofocus失效。
##### 2.通过id获取得到元素再focus
- 成功，在转换的时候，通过id获取得到相关元素，再将元素进行focus。
   ```
      this.$nextTick(() => {
        const input = document.getElementById("input");
        input.focus();
      });
   ```

#### 参考
- [手把手教你玩转render函数](https://juejin.cn/post/6969226302767235108)
