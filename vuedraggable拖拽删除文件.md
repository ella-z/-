# vuedraggable拖拽删除文件
- 问题背景：因需求，要实现将元素拖拽到指定位置，然后自动删除标签。
- 解决问题的过程：
   1. 首先考虑使用原生拖拽方法实现。
      - 在标签列表的vuedragable中绑定start方法。
        ```
           dragStart(e) {
              e.originalEvent.dataTransfer = null
              let id = e.srcElement.parentElement.id;
              let event = e.originalEvent;
              let item = this.x[e.oldIndex];
              let dt = event.dataTransfer;
              dt.effectAllowed = "all";
              dt.setData("item", JSON.stringify(item)); // 无法改变dataTransfer的值
           },
        ```
      - 指定区域trash-zone，绑定drop方法，接收拖拽到指定位置的方法。
        ```
           trashDrop(e) {
            let data = e.dataTransfer.getData("item");
            console.log(data, 123);
            if (!data) return;
            data = JSON.parse(data);
            console.log(data); // ''
          },
        ```
      - 运行代码的时候发现，trashDrop中的data为空，然后思路追溯到dragStart中，输出dt的值，发现dt内部属性的值并没有被改变。然后尝试修改dt，报错：TypeError: Cannot set property dataTransfer of #\<DragEvent> which has only a getter"。由此可知，在vuedraggable中的start事件中的无法改变dataTransfer的值，那么就会导致无法传递值，所以失败~
   2. 尝试使用两个vuedraggable区域互相拖拽实现
      - 标签列表保持不变。
      - trash-zone使用vuedraggable画出新的可拖拽区域。
        ```
          // vue
          <draggable v-model="trashData" class="trash-zone" :group="{ name: 'tagList', put: true, pull: false }">
                <div slot="footer" class="footer el-icon-delete"></div>
          </draggable>

          //css 主要实现标签拖拽到该处时能够实现标红效果，并且拖入的tag不显示
            .trash-zone {
                position: relative;
                display: inline-block;
                cursor: pointer;
                color: #b8babb;
                margin-right: 10px;
                .tag {
                  display: none;
                }
                .footer {
                  width: 35px;
                  height: 35px;
                  line-height: 35px;
                  text-align: center;
                  border: 1px dashed #b8babb;
                }
                .tag + .footer {
                  color: #f56c6c;
                  border-color: #f56c6c;
                  background-color: rgba($color: #f56c6c, $alpha: 0.1);
                }
            }
        ```
      - 成功实现~撒花❀  
      - 参考
         - [使用vuedraggable实现拖拽删除](https://mlog.club/article/1549391)
  
  
  
  
