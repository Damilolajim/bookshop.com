<template>
  <div class="bannerComp">{{ title }}</div>
</template>

<script>
export default {
  name: "bannerComp",
  props: {
    title: String,
  },
};
</script>

<style scoped>
.bannerComp {
  height: 30rem;
  background: #1a1a1a;
  color: #fff;
  font-size: 4rem;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
