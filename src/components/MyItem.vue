<template lang="">
  <div>
    <li>
      <label>
        <!-- change 改变,当内容改变时触发 -->
        <input type="checkbox" :checked="todo.done" @change="qvfan(todo.id)" />
        <span>{{ todo.title }}</span>
      </label>
      <button class="btn btn-danger" @click="todoDelete(todo.id)">删除</button>
    </li>
  </div>
</template>

<script>
export default {
  name: "MyItem",
  //用props接收list那边传出来的外部内容.props只能父子传递（嵌套）
  props: ["todo", "checkTodo",'deleteTodo'],
  methods: {
    qvfan(id) {
      console.log(id);
      //通知App组件将对应得done值取反
      this.checkTodo(id);
    },
    todoDelete(id){
      // confirm根据用户的交互觉得布尔值为真还是假
      if(confirm('你确定删除吗？')){
        this.deleteTodo(id)
      }
    }
  },
};
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-top: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover{
  background-color: rgb(228, 228, 228);
}

li:hover button{
  display: block;
}
</style>
