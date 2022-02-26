<template>
  <div class="name">{{ name }}, {{ array }}</div>
  <button class="btn btn-primary" @click="onClick">클릭</button>
</template>

<script>
// template의 변수가 동적인 변수로 바꾸는 하는 법
import { ref, reactive } from "vue"; // 1. ref를 import
export default {
  setup() {
    let name = ref("vue3"); // 2. 변수를 ref객체로 리턴
    let array = reactive([1, 2]); // 객체는 reactive를 사용하여 반응적인 객체복사본 리턴

    // 1. 함수 정의
    const onClick = () => {
      name.value = "is awesome"; // 3. ref처리한 변수의 값을 변경할 때는 변수.value에 접근해야함
      array.push(3);
      console.log(name, array);
    };
    return {
      name,
      array,
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
