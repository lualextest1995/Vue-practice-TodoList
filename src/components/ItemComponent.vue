<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.complete"
        @change="handlerCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        type="text"
        v-show="todo.isEdit"
        :value="todo.title"
        @blur="handlerBlur(todo, $event)"
        ref="inputFocus"
        :autofocus="todo.isEdit ? true : false"
      />
    </label>
    <button class="btn btn-delete" @click="handlerDelete(todo.id)">删除</button>
    <button
      class="btn btn-edit"
      @click="handlerEdit(todo)"
      v-show="!todo.isEdit"
    >
      編輯
    </button>
  </li>
</template>

<script>
import Swal from "sweetalert2";
import { inject, nextTick, ref } from "vue";
export default {
  name: "ItemComponent",
  props: ["todo"],
  setup() {
    const inputFocus = ref(null);
    //跨層級注入
    const updateTodoComplete = inject("updateTodoComplete");
    const deleteTodo = inject("deleteTodo");
    const editTodo = inject("editTodo");
    //checked回傳到資料所在位置
    function handlerCheck(id) {
      updateTodoComplete(id);
    }

    //刪除目標回傳到資料所在位置
    async function handlerDelete(id) {
      const result = await Swal.fire({
        title: "確定要刪除該代辦事項嗎？",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "刪除",
        cancelButtonText: "取消",
      });
      if (result.value) {
        deleteTodo(id);
      }
    }

    //編輯目標回傳到資料所在位置
    function handlerEdit(todo) {
      todo.isEdit = true;
      //重新渲染完，自動對焦
      nextTick(() => {
        inputFocus.value.focus();
      });
    }
    //失去焦點並回調修改資料
    function handlerBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) {
        return;
      }
      editTodo(todo.id, e.target.value);
    }

    return {
      inputFocus,
      handlerCheck,
      handlerDelete,
      handlerEdit,
      handlerBlur,
    };
  },
};
</script>

<style lang="scss" scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
  transition: all 0.3s ease;
  &:before {
    content: initial;
  }
  &:last-child {
    border-bottom: none;
  }
  &:hover {
    background-color: rgb(184, 176, 176);
    button {
      display: block;
    }
  }
  label {
    float: left;
    cursor: pointer;
    transition: all 0.3s ease;
    &:hover {
      color: orange;
    }
    input {
      vertical-align: middle;
      margin-right: 6px;
      position: relative;
      top: -1px;
    }
  }
  button {
    float: right;
    display: none;
    margin-top: 3px;
  }
}
</style>
