<template>
  <div class="name">{{ greet }}</div>
</template>

<script>
export default {
  setup() {
    const name = "vue3";

    // 2. 함수 정의
    const greeting = (name) => {
      return `hello ${name}`;
    };
    const greet = greeting(name);
    return {
      // 1. Obj에서 원하는 변수, 함수를 return
      //  -> template에서 접근 가능한 변수, 함수
      greet,
    };
  },
};
</script>

<style>
.name {
  color: red;
}
</style>
