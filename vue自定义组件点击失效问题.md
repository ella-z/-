# vue自定义组件点击失效问题
- 问题背景：在自定义的组件的父组件上给其添加绑定事件，可是发现并不能触发该事件。
- 问题的原因：点击事件作用于组件内部，若组件内部并没有点击事件，那就不会触发点击事件。
- 问题的解决方法：
  - 可以给点击事件添加.native即可以解决
    - 🌰：
    ```
       <RecomHotelCard
          class="hotel-card"
          @click.native="toDetail"
          :key="index"
          v-for="(item, index) in 10"
        ></RecomHotelCard>
    ```
