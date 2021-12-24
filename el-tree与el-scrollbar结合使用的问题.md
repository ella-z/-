# el-tree与el-scrollbar结合使用的问题
- 问题：
   1. 当el-tree的节点采用懒加载的方式进行节点的加载时，el-scrollbar的滚动条不会根据节点的变化进行更新，导致无法滚拖动滚动条进行滚动。
   2. 当x轴溢出的时候，横向滚动条却没有出现。
- 解决问题的过程：
   - 问题1：
      1. 找到el-scrollbar更新的方式：使用el-scrollbar中的update方法对scrollbar进行更新。
         ```
          🌰：
            this.$refs.scrollbar.update();
         ```
      2. 更新的方法知道了，那么在何时更新呢？
         - 第一种想法:在懒加载结束后，也就是el-tree的load方法return前，加入更新的方法，如下：
            ```
             this.$nextTick(()=>{
              this.$refs.scrollbar.update();
             })
            ```
            - 结果：失败，因为在节点展开完成前，就进行了滚动条更新，那么更新获取到的数据还是节点展开之前的。而且这种做法在节点折叠的时候无法触发滚动条更新。
         - 第二种想法：使用MutationObserver对树进行监控，当树的dom发生改变的时候触发回调函数，更新滚动条：
            ```
               const targetNode = document.getElementById("tree");
                const config = { attributes: true, childList: true, subtree: true };
                const callback = () => {
                  this.$refs.scrollbar.update();
                };
                const observer = new MutationObserver(callback);
                observer.observe(targetNode, config);
            ```
            - 结果：成功，撒花*★,°*:.☆(￣▽￣)/$:*.°★* 。
   - 问题2：需要将el-tree下第一层的node样式设置为：
      ```
         🌰：
         :deep(.el-tree > .el-tree-node) {
           display: inline-block;
           min-width: 100%;
         }
      ```
            
### 参考
- [MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
- [Element-ui el-scrollbar 源码解析](https://segmentfault.com/a/1190000019054618)
