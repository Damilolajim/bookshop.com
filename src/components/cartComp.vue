<template>
  <div class="course__listing">
    <slot>
      <div class="404">Your Cart is empty</div>
    </slot>
  </div>
</template>

<script>
export default {
  name: "cartComp",
};
</script>

<style scoped>
.course__listing {
  min-height: 50rem;
  display: flex;
  align-items: center;
}
</style>
