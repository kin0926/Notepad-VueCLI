<template lang="">
  <div>
    <li>
      <label>
        <input type="checkbox" :checked="todo.done" />
        <span>{{todo.title}}</span>
      </label>
      <button class="btn btn-danger" style="display: none">删除</button>
    </li>
  </div>
</template>

<script>
export default {
  name: "MyItem",
  //用props接收list那边传出来的外部内容.props只能父子传递（嵌套）
  props:['todo'],
  mounted(){
    console.log(this.todo);
  }
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
</style>
