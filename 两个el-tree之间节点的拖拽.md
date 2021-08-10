# 两个el-tree之间节点的拖拽
- 问题背景：需要将两个el-tree之间的节点自由拖拽，tree1的节点可以拖拽到tree2上，tree2的节点可以拖拽到tree1中。
- 问题的解决方法：
  - 一棵树拖拽到另一颗树的方法可以参考，大佬的方法：[两棵el-tree的节点跨树拖拽实现](https://blog.csdn.net/qq_41694291/article/details/108631887)
  - 我们需要对上面的这种方法进行改造，然后满足我们的需求。
  - 🌰：
    ```
      //两颗树上的allow-drop都要设置为true
      //methods：
        dTreeDragStart(node, event) {
          if (node.data.belong === "dTree") {
            this.$refs.mTree.$emit("tree-node-drag-start", event, { node: node });
          }
        },
        dTreeDragEnd(draggingNode, endNode, position, event) {
          //添加endNode && position判断，以防随意拖拽导致节点消失
          if (draggingNode.data.belong === "mTree" && endNode && position) {
            draggingNode.data.belong = "dTree";
            //当节点移动到新的树时，把原来的树上的节点删除掉
            this.$refs.mTree.remove(draggingNode.data.id);
          } else if (endNode && draggingNode.data.belong === endNode.data.belong) {
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
          if (draggingNode.data.belong === "dTree" && endNode && position) {
            draggingNode.data.belong = "mTree";
            this.$refs.dTree.remove(draggingNode.data.id);
          } else if (endNode && draggingNode.data.belong === endNode.data.belong) {
            this.$refs.dTree.$emit("tree-node-drag-end", event);
          }
        },
    ```
    
### 可拖拽树
- 将以上特性封装成为一个可拖拽的树。
  ```
    <template>
      <el-scrollbar wrap-class="scrollbar-wrapper" class="scrollbar">
        <el-tree
          :ref="treeRefName"
          node-key="id"
          :data="data"
          :allow-drop="() => true"
          :render-content="treeRenderContent"
          :expand-on-click-node="false"
          default-expand-all
          draggable
          @node-drag-start="treeDragStart"
          @node-drag-end="treeDragEnd"
          @node-contextmenu="nodeContextmenu"
        >
        </el-tree>
      </el-scrollbar>
    </template>

    <script>
    export default {
      props: {
        data: {
          type: Array,
          default: () => [],
        },
        targetTree: {
          type: Object,
          default: () => null,
        },
        treeRefName: {
          type: String,
          default: "elTree",
        },
        targetTreeName: {
          type: String,
          required: true,
        },
        outTreeTarget: {
          type: Boolean,
          default: false,
        },
        renderContent: {
          type: Function,
          required: true,
        },
      },
      data() {
        return {
          isMenuShow: false,
          currentDraggingNode: null,
          defaultProps: {
            children: "children",
            label: "label",
          },
        };
      },
      methods: {
        /**
         * 开始拖拽触发的函数
         * @param node 节点
         * @param event 事件
         */
        treeDragStart(node, event) {
          if (node.data.belong === this.treeRefName) {
            this.targetTree.$emit("tree-node-drag-start", event, { node: node });
            //将结点的数据添加到event.dataTransfer中进行保存
            let dt = event.dataTransfer;
            dt.effectAllowed = "all";
            node.data.isLeaf = node.isLeaf;
            dt.setData("item", JSON.stringify(node.data));
            this.currentDraggingNode = {
              parent: node.parent,
              nextSibling: node.nextSibling,
              previousSibling: node.previousSibling,
            };
          }
        },

        /**
         * 拖拽完成触发的函数
         * @param draggingNode 被拖拽的节点
         * @param endNode 结束节点
         * @param position 节点的最终位置
         * @param event 事件
         */
        treeDragEnd(draggingNode, endNode, position, event) {
          if (this.outTreeTarget) {
            //判断树节点是否在树区域外drop
            this.restartTree(draggingNode, this.treeRefName);
            this.$emit("update:outTreeTarget", false);
            this.currentDraggingNode = null;
            return;
          }

          //添加endNode && position判断，以防随意拖拽导致节点消失
          if (draggingNode.data.belong === this.targetTreeName && endNode && position) {
            this.changeRefName(draggingNode);
            //当节点移动到新的树时，把原来的树上的节点删除掉
            this.targetTree.remove(draggingNode.data.id);
          } else if ( draggingNode.data.belong === this.treeRefName) {
            //调用新树的dragEnd函数
            this.targetTree.$emit("tree-node-drag-end", event);
          }
        },

        /**
         * 修改节点的belong
         * @param node 节点
         */
        changeRefName(node) {
          node.data.belong = this.treeRefName;
          if (!node.isLeaf) {
            node.childNodes.forEach((item) => {
              this.changeRefName(item);
            });
          }
        },

        /**
         * 还原节点排序
         * @param draggingNode 被拖拽的节点
         * @param treeRef 树的ref
         */
        restartTree(draggingNode, treeRef) {
          let tree = this.$refs[treeRef];
          tree.remove(draggingNode.data);
          let { nextSibling, previousSibling } = this.currentDraggingNode;
          let _parent = this.currentDraggingNode.parent;
          if (nextSibling && previousSibling) {
            //当节点前后具有兄弟节点的时候，放置在前后节点之间
            tree.insertBefore(draggingNode.data, nextSibling);
          } else if (nextSibling && !previousSibling) {
            //当只有后兄弟节点的时候，放置在后节点的前面
            tree.insertBefore(draggingNode.data, nextSibling);
          } else if (!nextSibling && previousSibling) {
            //当只有前兄弟节点的时候，放置在前节点的后面
            tree.insertAfter(draggingNode.data, previousSibling);
          } else if (_parent && !nextSibling && !previousSibling) {
            //当没有前后节点的时候，直接拼接
            tree.append(draggingNode.data, _parent.data);
          }
        },
        nodeContextmenu(event, data, node) {
          this.$emit("contextmenu", { event, data, node });
        },
        treeRenderContent(h, obj) {
          const content = this.renderContent(h, obj);
          return content;
        },
      },
    };
    </script>

    <style lang="scss" scoped>
    .scrollbar-wrapper {
      width: 200px;
      height: 100%;
      overflow: hidden;
    }
    .scrollbar {
      padding: 0 0 20px 5px;
      height: 100%;
      .sidebar-icon {
        color: #409eff;
      }
      .tree-span {
        font-size: 15px;
      }
    }
    </style>
  ```
