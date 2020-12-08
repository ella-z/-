# el-tree懒加载+drag
- 问题:在el-tree中,需要将一个文件拖拽到一个还没有进行懒加载的文件夹中,会出现一种情况:当拖拽完成后,再打开文件夹的时候,发现拖拽进去的文件不见了.
- 原因:因为打开文件夹的时候,文件夹会进行懒加载,获取得到的数据会覆盖掉之前的数据,所以文件会不见.
- 解决问题的过程:
   - 总体思路:先将需要懒加载的文件加进行加载,加载完成后再进行文件的拖拽.
   - 第一次尝试:在allowDrop中,执行懒加载的函数loadNode,loadNode需要两个参数(Node,resolve), Node为allowDrop的第二个参数dropNode的值,resolve为第一层节点load的时候的值.
      - 结果:加载的值直接拼接到第一层中了,因为resolve是第一层节点的resolve.
   - 第二次尝试:在allowDrop中,对目标dropNode节点进行懒加载,进行数据的申请,然后将结果用append拼接到dropNode节点中,然后再将拖拽的节点赋值到目标节点中.
      - 结果:拖拽到懒加载文件夹成功,但是其他在allowDrop中的判断失效,因为需要异步对数据进行申请,所以需要将allowDrop设置为async,那么返回的数据,变成Promise.无法获取其中的数据,所以el-tree会将这些return都视为true,那么其他判断都失效了.
   - 第三次尝试:在allowDrop中先将文件赋值到目标节点.然后在node-drag-end中对数据进行申请,再将申请得到的数据append到目标节点中.
      - 结果:成功~

