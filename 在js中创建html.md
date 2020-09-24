# 在js中创建html
- 问题：因需求，需要触发某个条件后再创建相应的html并渲染，类似于在ant-design-vue 的table中创建动态的html并渲染到table中。
- 解决：
   - 使用JSX语法，当触发方法的时候，返回html，页面再进行渲染。
   - 在vue中有babel-plugin-transform-vue-jsx插件可以实现在vue中进行jsx语法编写。
   ```
   🌰：
   <script lang="tsx">
     ......
       customRender: (value:any) => {
             todo something....
             return (<a-button type="link">{value}</a-button>);
        }
     ......
    </script>
   ```
 - 坑：
    - 在配置babel.config.js时报错：Duplicate declaration "h" (This is an error on an internal node. Probably an internal error.)
    - 原因：在@vue/cli V3，不用配置可以直接用，否则配置后会报重复声明错误。
    - 参考：https://www.cnblogs.com/givingwu/p/10220295.html
 - 资料：
    - [babel-plugin-transform-vue-jsx](https://github.com/vuejs/babel-plugin-transform-vue-jsx)
    - [在Vue中使用JSX的正确姿势](https://zhuanlan.zhihu.com/p/37920151)
