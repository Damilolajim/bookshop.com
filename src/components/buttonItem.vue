<template>
  <a :class="getClassName" :href="link">{{ text }}</a>
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
      return `btn ${this.className}`;
    },
  },
};
</script>

<style>
.btn {
  text-decoration: none;
  color: currentColor;
  display: inline-block;
  padding: 1rem 2rem;
}

.btn__block {
  display: block;
}

.btn__primary {
  background: red;
  color: black;
}
</style>
