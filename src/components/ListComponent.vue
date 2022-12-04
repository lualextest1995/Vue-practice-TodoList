<template>
  <ul class="main">
    <transition-group
      name="animate__animated animate__bounce"
      enter-active-class="animate__fadeInDown"
      leave-active-class="animate__fadeOutLeft"
      appear
    >
      <ItemComponent
        v-for="todo in todoList"
        :key="todo.id"
        :todo="todo"
      ></ItemComponent>
    </transition-group>
  </ul>
</template>

<script>
import "animate.css";

import ItemComponent from "./ItemComponent.vue";
export default {
  name: "ListComponent",
  props: ["todoList"],
  components: { ItemComponent },
};
</script>

<style lang="scss" scoped>
.main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
  overflow: auto;
  max-height: 700px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
