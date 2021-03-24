# 两个el-tree之间节点的拖拽
- 问题背景：需要将两个el-tree之间的节点自由拖拽，tree1的节点可以拖拽到tree2上，tree2的节点可以拖拽到tree1中。
- 问题的解决方法：
  - 一棵树拖拽到另一颗树的方法可以参考，大佬的方法：[两棵el-tree的节点跨树拖拽实现](https://blog.csdn.net/qq_41694291/article/details/108631887)
  - 我们需要对上面的这种方法进行改造，然后满足我们的需求。
  - 🌰：
    ```
      //两颗树上的allow-drop都要设置为true
      dTreeDragStart(node, event) {
        if (node.data.belong === "dTree") {
          this.$refs.mTree.$emit("tree-node-drag-start", event, { node: node });
        }
      },
      dTreeDragEnd(draggingNode, endNode, position, event) {
        //同一颗树上的拖拽，直接执行即可
        if (draggingNode.data.belong === endNode.data.belong) return;
        if (draggingNode.data.belong === "mTree" && endNode && position) {
          draggingNode.data.belong = "dTree";
          //当节点移动到新的树时，把原来的树上的节点删除掉
          this.$refs.mTree.remove(draggingNode.data.id);
        } else {
          //调用新树的dragEnd函数
          this.$refs.mTree.$emit("tree-node-drag-end", event);
        }
      },
      mTreeDragStart(node, event) {
        if (node.data.belong === "mTree") {
          this.$refs.dTree.$emit("tree-node-drag-start", event, { node: node });
        }
      },
      mTreeDragEnd(draggingNode, endNode, position, event) {
        if (draggingNode.data.belong === endNode.data.belong) return;
        if (draggingNode.data.belong === "dTree" && endNode && position) {
          draggingNode.data.belong = "mTree";
          this.$refs.dTree.remove(draggingNode.data.id);
        } else {
          this.$refs.dTree.$emit("tree-node-drag-end", event);
        }
      }
    ```
