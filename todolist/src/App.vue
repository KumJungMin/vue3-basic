<template>
  <div class="name">{{ name }}</div>
  <input type="text" :value="name" @input="updateName" />
  <button class="btn btn-primary" @click="onClick">클릭</button>
</template>

<script>
// 양방향 바인딩하는 법
import { ref } from "vue";
export default {
  setup() {
    let name = ref("vue3");

    const onClick = () => {
      name.value = "is awesome";
    };
    const updateName = (e) => {
      name.value = e.target.value;
    };
    return {
      name,
      onClick,
      updateName,
    };
  },
};
</script>

<style>
.name {
  color: red;
}
</style>
