<template lang="">
  <div>
    <div class="todo-header">
      <input
        type="text"
        placeholder="请输入你的任务名称，按回车键确认" 
        v-model="title"
        @keyup.enter="add"
      />
    </div>
  </div>
</template>

<script>
// 调用nanoid
import { nanoid } from "nanoid";

export default {
  name: "MyHeader",
  props:['addTodo'],
  data() {
    return {
      title:''
    }
  },
  methods: {
    add() {
      // 也可以加上在!this.title后加上.trim()，即去掉前后空格
      if(!this.title){return alert('不能为空')}
      // console.log(e.target.value);
      // uuid的精简版nanoid,安装npm i nanoid
      // 将用户输入的内容包装成一个todo对象
      let todoObj = { id: nanoid(), title: this.title, done: false };
      // 通知App组件添加一个todoObj对象
      this.addTodo(todoObj);
      // 清空输入框
      this.title=''
    },
  },
};
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
