<template>
  <div class="name">{{ name }}</div>
  <input type="text" v-model="name" />
  <!-- v-model: 변수 바인딩 + 변수 변경 이벤트 -->
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
