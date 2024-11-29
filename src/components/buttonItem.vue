<template>
  <a :class="getClassName()" :href="link">{{ text }}</a>
</template>

<script>
export default {
  name: "buttonItem",
  props: {
    text: String,
    className: String,
    link: String,
  },
  methods: {
    getClassName() {
      return `btn btn__block ${this.className}`;
    },
  },
};
</script>

<style scoped>
.btn {
  text-decoration: none;
  color: currentColor;
  display: inline-block;
  padding: 1rem 0.8rem;
  text-align: center;
  font-size: 1.3rem;
}

.btn__block {
  display: block;
}

.btn__primary {
  background: #333333;
  color: #fff;
}
</style>
