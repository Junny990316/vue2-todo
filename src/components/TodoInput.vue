<template>
  <div class="inputBox shadow">
    <input
      type="text"
      v-model="newTodoItem"
      ref="todoItem"
      @keyup.enter="addTodo"
    />
    <!-- <button v-on:click="addTodo">추가</button> -->
    <span class="addContainer" v-on:click="addTodo">
      <i class="fas fa-plus addBtn"></i>
    </span>
    <MyModal v-if="showModal" @close="showModal = false">
      <h3 slot="header">
        경고!
        <i class="closeModalBtn fas fa-times" @click="showModal = false"></i>
      </h3>
      <div slot="body">아무것도 입력하지 않으셨습니다.</div>
    </MyModal>
  </div>
</template>

<script>
import MyModal from "@/components/common/MyModal.vue";
export default {
  data() {
    return {
      // 변수 선언
      newTodoItem: "",
      showModal: false,
    };
  }, // data

  components: {
    MyModal,
  }, // components
  //LifeCycle Hook Method
  //$refs는 document.getElementById(id) 함수처럼 html dom에 직접 접근할 때 사용되는 객체
  //ref="todoItem"의 ref 속성은 기존의 id 속성과 동일한 속성이다
  mounted() {
    this.$refs.todoItem.focus();
  },
  methods: {
    addTodo() {
      if (this.newTodoItem !== "") {
        this.$emit("addTodoEvent", this.newTodoItem);
        this.clearInput();
      } else {
        this.showModal = !this.showModal;
      }
    },
    clearInput() {
      this.newTodoItem = "";
    }, // clearInput
  }, // methods
};
</script>

<style scoped>
i {
  cursor: pointer;
}
input:focus {
  outline: none;
}
.inputBox {
  background: white;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
  width: 75%;
  height: 50%;
}
.addContainer {
  float: right;
  background: linear-gradient(to right, #6478fb, #8763fb);
  display: block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}
.addBtn {
  color: white;
  vertical-align: middle;
}
.closeModalBtn {
  color: #42b983;
}
</style>
