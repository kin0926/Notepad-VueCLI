<template lang="">
  <div>
    <li>
      <label>
        <!-- change 改变,当内容改变时触发 -->
        <input type="checkbox" :checked="todo.done" @change="qvfan(todo.id)" />
        <span v-show="!todo.isEdit">{{ todo.title }}</span>
        <input
          type="text"
          v-show="todo.isEdit"
          :value="todo.title"
          @blur="todoBlur(todo,$event)"
          ref="inputTitle"
        />
      </label>
      <button class="btn btn-danger" @click="todoDelete(todo.id)">删除</button>
      <button class="btn btn-edit" v-show="!todo.isEdit" @click="todoEdit(todo)">编辑</button>
    </li>
  </div>
</template>

<script>
export default {
  name: "MyItem",
  //用props接收list那边传出来的外部内容.props只能父子传递（嵌套）
  props: ["todo"],
  methods: {
    qvfan(id) {
      console.log(id);
      //通知App组件将对应得done值取反
      // this.checkTodo(id);
      this.$bus.$emit("checkTodo", id);
    },
    // 删除
    todoDelete(id) {
      // confirm根据用户的交互觉得布尔值为真还是假
      if (confirm("你确定删除吗？")) {
        // this.deleteTodo(id)
        this.$bus.$emit("deleteTodo", id);
      }
    },
    // 编辑
    todoEdit(todo) {
      // 用$set方法给对象【todo】 添加一个属性【isEdit】 值为 【true】
      // 注意：上面用了两个v-show，第一个是!xxx,也就是false，所以会先显示第一个
      // 当我们点击按钮时给todo添加了isEdit，值是true，这时候!xxx就会隐藏，换成下面的显示
      if (Object.prototype.hasOwnProperty.call(todo, 'isEdit')) {
        todo.isEdit = true
      } else{
      this.$set(todo, 'isEdit', true);
      console.log(todo);
      console.log('没有isRdit，添加完毕');
      }
      // 点击编辑后获取焦点
      // 数据改变→dom重新渲染→nextTick等到渲染完成后(即下一次)，最后才起作用
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
    },
    // 失去焦点，编辑完毕，实现内容修改。把isEdit换回false，也就是第一个显示的状态
    todoBlur(todo,e){
      todo.isEdit = false
      this.$bus.$emit('updateTodo',todo.id,e.target.value)
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

li:hover {
  background-color: rgb(228, 228, 228);
}

li:hover button {
  display: block;
}
</style>
