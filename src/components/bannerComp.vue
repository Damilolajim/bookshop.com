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
  height: 40rem;
  background: linear-gradient(to bottom right, #666666bd, #1a1a1abf),
    linear-gradient(to top right, #1a1a1ad0, #666666c9);
  color: #cccccc;
  font-size: 4rem;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
  font-weight: 600;
}
</style>
