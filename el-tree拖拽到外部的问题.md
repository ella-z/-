# el-tree拖拽到外部的问题
- 问题背景：拖拽el-tree的树节点至树以外某个区域的时候，因为el-tree内部本身就有拖拽事件，然后在拖拽节点到外部某个区域的时候，可能会触发树本身的拖拽，导致树节点的位置在本身树的区域改变了。
- 解决问题的方法：
   1. 先声明一个对象，存储树节点拖拽前的位置，🌰：
      ```
        this.currentDraggingNode = {
          parent1: node.parent,
          nextSibling: node.nextSibling,
          previousSibling: node.previousSibling,
        };
      ```
   2. 在区域内监控drop，若触发了drop函数则使用一个全局变量标记，这个行为是节点拖拽到外部了
      ```
        this.outTreeTarget = true;
      ```
   3. 在拖拽结束的时候，将拖拽的节点插入到原来的位置
      ```
        dragEnd(draggingNode, endNode, position, event) {
            if (this.outTreeTarget) {
              this.restartTree(draggingNode, "mTree");
              this.outTreeTarget = false;
              this.currentDraggingNode = null;
              return;
            }
        }
        
        restartTree(draggingNode, treeRef) {
          //还原节点排序
          let tree = this.$refs[treeRef];
          tree.remove(draggingNode.data);
          let { nextSibling, previousSibling } = this.currentDraggingNode;
          let _parent = this.currentDraggingNode.parent1;
          if (nextSibling && previousSibling) {
            tree.insertBefore(draggingNode.data, nextSibling);
          } else if (nextSibling && !previousSibling) {
            tree.insertBefore(draggingNode.data, nextSibling);
          } else if (!nextSibling && previousSibling) {
            tree.insertAfter(draggingNode.data, previousSibling);
          } else if (_parent && !nextSibling && !previousSibling) {
            tree.append(draggingNode.data, _parent.data);
          }
        }
      ```
