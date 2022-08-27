<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList
          :todos="todos"
          :checkTodo="checkTodo"
          :deleteTodo="deleteTodo"
        ></MyList>
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
