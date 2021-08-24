# 数组改变el-table视图不变
- 问题：在创建动态表头的表格中，表头数组改变了，但视图并没有更新。
- 解决的方法：在table标签中添加一个随机key，若需要更新表格视图，那就重新赋值一下key。
  ```
    <el-table :key="currentKey" :data="tableData">
      <el-table-column
        :key="item.id"
        v-for="item in columnData"
        :prop="item.label"
        :label="item.label"
        min-width="150"
      ></el-table-column>
    </el-table>
    
    <script>
      export default {
        props:{
          obj:{
            type:Object,
          }
        },
        data(){
          return{
            currentKey:Math.random();
          }
        },
        computed:{
          columnData(){
            this.currentKey = Math.random();
            return obj.columnData;
          }
        }
      }
    </script>
  ```
