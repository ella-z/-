# iframe自适应高度(同域)的踩坑之路
- 问题背景：因需求，需要以弹框内嵌iframe的形式显示数据并且需要完整显示子页面，也就是说要让子页面把父iframe给撑开。
- 解题思路：
   - 第一次尝试:
      - 想法：在父iframe中获取子页面的高度，然后再将子页面的高度赋值给iframe，使得iframe能完整地展示子页面无需滚动。
      
      ```
          const iframe = this.$refs.iframe;
          iframe.style.height = this.$refs.iframe.contentWindow.document.body.scrollHeight + "px";
      ```
      
      - 结果：失败，因为子页面中有个异步请求，所以iframe初步渲染的时候只获取了子页面的还未返回数据的高度。
   - 第二次尝试：
      - 想法：在子页面中异步获取数据之后，计算子页面的高度，然后将子页面的高度赋值给父iframe，让父iframe能完整展示子页面无需滚动。
      ```
      //子页面中
        this.$nextTick(() => {
          const height = this.$refs.xxx.offsetHeight;
          this.resetParentWindowHeight(height);
        });
        resetParentWindowHeight(heightValue) {
          if (
            window.parent !== window.self &&
            self.frameElement !== null &&
            self.frameElement.getAttribute("id") !== null
          ) {
            const iframeId = self.frameElement.getAttribute("id");
            const iframe = window.parent.document.getElementById(iframeId);
            if (iframe !== null) {
              iframe.width = "100%";
              iframe.height = heightValue;
            }
          }
        }
      ```
      - 成功~
 
### 跨域情况下的iframe自适应高度
- 跨域情况下的iframe自适应高度，跨域情况下的自适应高度可以使用postMessage进行高度的传递。
- 可参考文章：
   - [前端爬坑日记之vue内嵌iframe并跨域通信](https://segmentfault.com/a/1190000016258735?utm_source=tag-newest)



