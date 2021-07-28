# 实现css变量
#### 问题的产生
- 因需求，需要实现根据父组件传递过来的颜色，对子组件的颜色进行赋值。
#### 问题解决的过程
- 使用css变量，接收父组件传递过来的颜色，然后在赋值给不同的css属性。
  ```
    <div class="tag" :style="`--tag-color:${tagColor}`"></div>
    //scss中
    .tag{
      color: var(--tag-color);
      border: 1px dashed var(--tag-color);
    }
  ```

#### 参考
- [妙用CSS变量，让你的CSS变得更心动](https://juejin.cn/post/6844904084936327182#heading-0)
