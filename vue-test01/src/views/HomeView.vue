<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Hi, gay!" />
    <Calculator />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import Calculator from '@/components/Calculator.vue';

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    Calculator,
  },
};
</script>
