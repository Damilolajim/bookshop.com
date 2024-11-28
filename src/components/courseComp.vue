<template>
  <div class="course__listing">
    <slot>
      <div class="404">Your Courses are empty</div>
    </slot>
  </div>
</template>

<script>
export default {
  name: "courseComp",
};
</script>

<style scoped>
.course__listing {
  min-height: 50rem;
  display: flex;
  align-items: center;
}
</style>
