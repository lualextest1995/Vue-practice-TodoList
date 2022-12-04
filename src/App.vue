<template>
  <div class="container">
    <div class="wrap">
      <h1>TodoList</h1>
      <AddComponent @addTodo="addTodo"></AddComponent>
      <ListComponent :todoList="todoList"></ListComponent>
      <FooterComponent
        :todoList="todoList"
        @allChecked="allChecked"
        @deleteCompletedTodo="deleteCompletedTodo"
      ></FooterComponent>
    </div>
  </div>
</template>

<script>
import { onMounted, ref, watch, provide } from "vue";
import AddComponent from "./components/AddComponent.vue";
import ListComponent from "./components/ListComponent.vue";
import FooterComponent from "./components/FooterComponent.vue";

export default {
  name: "App",
  components: { AddComponent, ListComponent, FooterComponent },
  setup() {
    const todoList = ref([]);
    //初始化載入 localStorage 的 Data
    onMounted(() => {
      todoList.value = JSON.parse(localStorage.getItem("data"));
    });
    //監控data改變時，更新 localStorage 裡的 Data
    watch(
      todoList,
      (value) => {
        localStorage.setItem("data", JSON.stringify(value));
      },
      { deep: true }
    );
    //跨層級提供
    provide("updateTodoComplete", updateTodoComplete);
    provide("deleteTodo", deleteTodo);
    provide("editTodo", editTodo);
    //新增代辦事項
    function addTodo(obj) {
      todoList.value = [obj, ...todoList.value];
    }
    //代辦事項完成勾選更新
    function updateTodoComplete(id) {
      todoList.value.forEach((item) => {
        if (item.id === id) {
          item.complete = !item.complete;
        }
      });
    }
    //代辦事項刪除更新
    function deleteTodo(id) {
      todoList.value = todoList.value.filter((item) => {
        return item.id !== id;
      });
    }
    //代辦事項全部勾選
    function allChecked(boolean) {
      todoList.value.forEach((item) => {
        item.complete = boolean;
      });
    }
    //刪除所有已完成的代辦事項
    function deleteCompletedTodo() {
      todoList.value = todoList.value.filter((item) => {
        return item.complete !== true;
      });
    }
    //編輯更新資料
    function editTodo(id, value) {
      todoList.value.forEach((item) => {
        if (item.id === id) {
          item.title = value;
        }
      });
    }

    return {
      todoList,
      addTodo,
      updateTodoComplete,
      deleteTodo,
      allChecked,
      deleteCompletedTodo,
      editTodo,
    };
  },
};
</script>

<styl lang="scss">
body {
  background: rgb(155, 185, 214);
  display: flex;
  justify-content: center;
  align-items: center;
  .container {
    background: rgb(117, 164, 209);
    width: 600px;
    margin: 0 auto;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    .wrap {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
      h1 {
        text-align: center;
        color: rgb(255, 0, 0);
      }
    }
  }
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
  &:focus {
    outline: none;
  }
}

.btn-delete {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
  &:hover {
    color: #fff;
    background-color: #bd362f;
  }
}

.btn-edit {
  color: #fff;
  background-color: #33e5e2;
  border: 1px solid #1fb9b7;
  margin-right: 5px;
  &:hover {
    color: #fff;
    background-color: #29c7c5;
  }
}
</styl>
