# transition标签问题
- 问题：使用多层transition标签以及v-show只设置在外div中，出现只有最外层transition标签产生的效果。
- 原因：transition要与v-show配合使用，transition包含的div中要有v-show搭配使用，才能触发transition的效果。
