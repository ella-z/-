# element树与vuedraggable之间的拖拽
- 问题背景：因需求，需要实现element-tree中的叶节点能拖拽进vuedraggable的区域内，因为采用的是两种不同的组件，所以它们之间并不能直接拖拽。
- 问题的解决方法：
  - 使用原生的js实现
    1. 首先在element-tree中的node-drag-start方法中将，拖拽元素的数据添加到event.dataTransfer中。
      ```
        let dt = event.dataTransfer;
        dt.effectAllowed = "all";
        dt.setData("item", JSON.stringify(data));
      ```
    2. 在vuedraggable的父元素中添加drop函数，作为接收元素的信息，然后在指定数组中添加元素。
      ```
        e.dataTransfer.getData("item");
      ```
    3. 成功完成需求~
    - 这种方法的缺点是：在拖拽的时候，不可以直接进行排序，还需要进一步升级。
  - 参考：
    - [HTML5之原生拖拽](https://blog.csdn.net/qq_41694291/article/details/101384209?spm=1001.2014.3001.5501)  
