<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @addTodoEvent="addTodo"></TodoInput>
    <TodoList :todo-data="todoItems" 
              @removeTodoEvent="removeTodo"
              @togleTodoEvent="togleTodo"></TodoList>
    <TodoFooter @clearTodoEvent="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "@/components/TodoHeader.vue";
import TodoInput from "@/components/TodoInput.vue";
import TodoList from "@/components/TodoList.vue";
import TodoFooter from "@/components/TodoFooter.vue";

export default {
  name: "App",
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      todoItems: [],
    };
  }, // data
  methods: {
    addTodo(todoItem) {
      var todoObj = { completed: false, item: todoItem };
      // stringify() 함수 json -> object로 변환
      localStorage.setItem(todoItem, JSON.stringify(todoObj));
      this.todoItems.push(todoObj);
    },
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    togleTodo(todoItem, index) {
      const { item, completed } = todoItem;
      this.todoItems[index].completed = !completed;
      localStorage.removeItem(item);
      localStorage.setItem(item, JSON.stringify(this.todoItems[index]));
    },
    clearTodo() {
      localStorage.clear();
      this.todoItems = []; // 배열도 초기화 시켜주기
    },
  },
  /* life cycle method */
  created() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        var itemJson = localStorage.getItem(localStorage.key(i));
        // parse() 함수 : json -> object로 변환
        this.todoItems.push(JSON.parse(itemJson));
      }
    }
  },
};
</script>

<style src="@/assets/styles/styles.css"></style>
