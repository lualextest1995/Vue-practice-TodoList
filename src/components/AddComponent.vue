<template>
  <div class="add">
    <input
      type="text"
      placeholder="請輸入你的代辦事項並按下Enter鍵傳送！！"
      v-model="data"
      @keyup.enter="submit"
    />
  </div>
</template>

<script>
import { ref } from "vue";
import { nanoid } from "nanoid";
import Swal from "sweetalert2";
export default {
  name: "AddComponent",
  setup(props, { emit }) {
    const data = ref("");

    function submit() {
      if (data.value === "") {
        return Swal.fire({
          title: "輸入欄不能為空!",
          icon: "error",
          confirmButtonText: "OK",
        });
      }
      const todoObj = {
        id: nanoid(),
        title: data.value,
        complete: false,
      };
      emit("addTodo", todoObj);
      //enter後清空字串
      data.value = "";
    }
    return { submit, data };
  },
};
</script>

<style lang="scss" scoped>
.add {
  input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
    &:focus {
      outline: none;
      border-color: rgba(82, 168, 236, 0.8);
      box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
        0 0 8px rgba(82, 168, 236, 0.6);
    }
  }
}
</style>
