<template lang="">
  <div>
    <div class="todo-footer" v-show="tatle">
      <label>
        <input type="checkbox" :checked="isAll" @change='checkAll'/>
      </label>
      <span> <span>已完成{{doneTotal}}</span> / 全部{{tatle}} </span>
      <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props:['todos','checkAllTodo','clearAllTodo'],
  computed:{
    tatle(){
      return this.todos.length
    },
    doneTotal(){
      let i = 0;
      this.todos.forEach(todo => {
        if(todo.done) i++
      });
      return i
      // 方法2 用reduce
      // return this.todos.reduce((pre,todo) => pre+(todo.done ? 1:0),0)
    },
    isAll(){
      return this.doneTotal == this.tatle && this.tatle > 0
    },
  },
  methods:{
    // 全选or取消全选
      checkAll(e){
        this.checkAllTodo(e.target.checked)
      },
      //清空
      clearAll(){
        this.clearAllTodo()
      }
    }
};
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
