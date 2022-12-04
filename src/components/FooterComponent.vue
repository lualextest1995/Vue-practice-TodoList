<template>
  <div class="todo-footer" v-show="total > 0">
    <label>
      <input type="checkbox" v-model="allCompleted" />
    </label>
    <span>
      <span>已完成{{ completeTotal }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-delete" @click="clearAll">清除已完成代辦事項</button>
  </div>
</template>

<script>
import { computed } from "vue";
import Swal from "sweetalert2";
export default {
  name: "FooterComponent",
  props: ["todoList"],
  setup(props, { emit }) {
    //計算完成數量
    const completeTotal = computed(() => {
      return props.todoList.reduce((prev, item) => {
        return prev + (item.complete ? 1 : 0);
      }, 0);
    });

    //計算全部數量
    const total = computed(() => props.todoList.length);
    //計算代辦事項是否都完成
    const allCompleted = computed({
      get: () => completeTotal.value === total.value && total.value > 0,
      //全部完成選項
      set: (value) => {
        emit("allChecked", value);
      },
    });

    //傳送刪除所有已完成的代辦事項
    async function clearAll() {
      const result = await Swal.fire({
        title: "確定要刪除所有已完成的代辦事項嗎？",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "刪除",
        cancelButtonText: "取消",
      });
      if (result.value) {
        emit("deleteCompletedTodo");
      }
    }

    return { completeTotal, total, allCompleted, clearAll };
  },
};
</script>

<style lang="scss" scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
  label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
    input {
      position: relative;
      top: -1px;
      vertical-align: middle;
      margin-right: 5px;
    }
  }
  button {
    float: right;
    margin-top: 5px;
  }
}
</style>
