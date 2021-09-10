# 根据不同的条件显示不同的Html
- 根据不同的条件显示不同的Html。
#### 1. 通过v-if或者v-show
- 对于类型少以及复杂度小的情况可以使用这种方法，简单便捷。
  ```
    <el-form>
      <template v-if="type==='1'||type==='2'">
         <el-form-item label="name">
              <el-input v-model="name"></el-input>
         </el-form-item>
      </template>
      <template v-else>
         ......
      </template>
    </el-form>
  ```
- 缺点：当类型多、复杂度大时，代码量会变得很大并且不易理解
  ```
     <el-form>
      <template v-if="...">
         ......
      </template>
          .
          .
      <template v-else-if="...">
         ......
      </template>
          .
          .
      <template v-else>
         ......
      </template>
    </el-form>
  ```

#### 2. 使用component组件与:is配合使用
- 这种方式适合用于根据条件展示不同的组件，但要声明组件。
  ```
    <component :is="type"></component>
    <script>
      export default {
        components: {
          component1:()=>import("./components/one")
        }
        data:{
          type:'component1'
        }
      }
    </script>
  ```
- 缺点：这种方法不适用于重复性比较高的组件，比如在el-form中，有三种不同的type，每种type中都包含了同一个el-form-item，那么就要在这三个组件中都重写一遍这个el-form-item。

#### 3. 使用JSX
- 使用jsx匹配的方式进行组件形成组件。
  ```
    // 在源文件中
    <template>
      <el-form>
        <formItem></formItem>
      <el-form>
    </template>
    
    <script>
      import FormItem from "./formItem.vue"
      export default {
        components:{
          FormItem
        }
        data(){
          return{
              name:{
                type:'input',
                label:'名字'
                value:''
              }，
              isShowName:{
                type:'checkbox',
                label:'显示名字',
                value: false
              },
              profession:{
                type:'profession',
                label:'职业'
                value:'teacher'
             }
          }
        }
      }
    </script>
    
    // formItem
    <script>
      const items = {
        checkbox(key) {
          return (
            <el-form-item>
              <el-checkbox
                value={this.form[key].value}
                onChange={(val) => {
                  this.form[key].value = val;
                }}
              >
                {this.form[key].label}
              </el-checkbox>
            </el-form-item>
          );
        },
        input(key) {
          return (
            <el-form-item label={this.form[key].label}>
              <el-input
                value={this.form[key].value}
                size="mini"
                placeholder={`请输入${this.form[key].label}`}
                onInput={(val) => {
                  this.form[key].value = val;
                }}
              ></el-input>
            </el-form-item>
          );
        },
        profession(key){
          const professionTypes = [
            {
              label: "老师",
              value: "teacher",
            },
            {
              label: "医生",
              value: "doctor",
            },
          ];
          return (
            <el-form-item label="职业">
              <el-select
                value={this.form[key].value}
                placeholder="请选择"
                size="mini"
                onInput={(val) => {
                  this.form[key].value = val;
                }}
              >
                {professionTypes.map((item) => {
                  return <el-option value={item.value} label={item.label}></el-option>;
                })}
              </el-select>
            </el-form-item>
          );
        }
      }
      export default {
        props: {
          form: {
            type: Object,
            requried: true,
          },
        },
        render(h) {
          if (!this.form) {
            return;
          }
          let keys = Object.keys(this.form);
          let components = keys.map((key) => {
            return items[this.form[key].type].call(this, key);
          });
          if (components) {
            return <el-col>{components}</el-col>;
          }
        },
      };
    </script>
    
  ```

