<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todos"></MyList>
        <MyFooter
          :todos="todos"
          @checkAllTodo="checkAllTodo"
          @clearAllTodo="clearAllTodo"
        ></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyList from "./components/MyList.vue";
import MyFooter from "./components/MyFooter.vue";
// 不用引入item，因为item是list的子组件
// import MyItem from './components/MyItem.vue';

export default {
  name: "App",
  components: {
    MyHeader,
    MyList,
    MyFooter,
  },
  data() {
    return {
      // todos: [
      //   { id: "001", title: "请输入内容并回车", done: false },
      //   JSON.parse(localStorage.getItem("todos")),
      // ],

      //监听事件里把历史事件存到本地存储里了，这里调用本地存储以实现记事本内容不随刷新而消失
      //第一次使用时没有内容，可以在后面添加个‘ || [] ’空数组
      todos: JSON.parse(localStorage.getItem("todos")) || []
    };
  },
  methods: {
    //添加一个todo
    addTodo(todoObj) {
      console.log("App.vue收到了数据：" + todoObj);
      this.todos.unshift(todoObj);
    },
    //取反对应todo
    checkTodo(id) {
      // forEach遍历
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.done = !todo.done;
        }
      });
    },
    // 删除一个todo
    deleteTodo(id) {
      // filter过滤
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id;
      });
    },
    // 更新编辑后todo
    updateTodo(id,title){
      this.todos.forEach((todo) => {
        if (todo.id === id) {todo.title = title;}
      });
      
    },
    //全选or全不选
    checkAllTodo(done) {
      this.todos.forEach((todo) => {
        todo.done = done;
      });
    },
    //清除所有已完成的todo
    clearAllTodo() {
      // filter过滤
      this.todos = this.todos.filter((todo) => {
        return !todo.done;
      });
    },
  },
  watch: {
    //简写的话只能浅监视，这里得用完整写法+deep:true开启深度监视，才能知道事件前面的框是否勾选‘完成’
    todos:{
      deep:true,
      handler(value){
        localStorage.setItem("todos", JSON.stringify(value));
        }
    }
  },
  mounted(){
    // 只要加载完毕就找到bus这个事件总线，on绑定上去
    this.$bus.$on('checkTodo',this.checkTodo)
    this.$bus.$on('deleteTodo',this.deleteTodo)
    this.$bus.$on('updateTodo',this.updateTodo)
  },
  beforeDestroy(){
    // bus销毁时-解绑-上面那两个
    this.$bus.$off('checkTodo')
    this.$bus.$off('deleteTodo')
    this.$bus.$off('updateTodo')
  }
};
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-edit{
  color: #fff;
  background-color: #2c8630;
  border: 1px solid #000000;
  margin-right: 7px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
