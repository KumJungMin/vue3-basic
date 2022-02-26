<template>
  <div class="name">{{ name }}</div>
  <input type="text" :value="name" />
  <button class="btn btn-primary" @click="onClick">클릭</button>
</template>

<script>
// bind 쓰는 법
import { ref } from "vue";
export default {
  setup() {
    let name = ref("vue3");

    const onClick = () => {
      name.value = "is awesome";
    };
    return {
      name,
      onClick,
    };
  },
};
</script>

<style>
.name {
  color: red;
}
</style>
