# elementUI中message失效的问题
- 问题：在element组件dialog中使用message，发现message被dialog覆盖掉了，显示不了。
- 解决：message中有customClass用来定义类，可以通过这个定义类来定义message的z-index，使得message不再被dialog覆盖。
   - 注意：该定义类不能放在scoped中，否则定义的样式类不生效。
```
🌰：
  <script>
     this.$message({
          message: "xxxx",
          type: "error",
          customClass: "messageZIndex",
      });
   </script>
   <style lang="scss">
      .messageZIndex {
        z-index: 30005 !important;
      }
    </style>   
```
