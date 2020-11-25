# vue中修改data中的数组和对象问题
- 在vue中修改data中的数组和对象，但是dom不刷新。
- 解决方法的过程：
   1. 先在js中输出看修改是否已生效？生效~
   2. 百度发现Vue 不能检测以下变动的数组：
      - 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
      - 当你修改数组的长度时，例如：vm.items.length = newLength
   3. 
      - 解决方法一:vm.items.splice(indexOfItem, 1, newValue)
      - 解决方法二:this.$set(vm.items,indexOfItem,newValue)
   4. 完美解决~

