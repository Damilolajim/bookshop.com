<template>
  <nav class="navbar">
    <div class="container">
      <ul class="navbar__wrap">
        <li class="navbar__item">
          <a
            href="#"
            :class="getClassName('home')"
            @click.prevent="navigate('home')"
            >Home</a
          >
        </li>
        <li class="navbar__item">
          <a
            href="#"
            :class="getClassName('cart')"
            @click.prevent="navigate('cart')"
            >Cart</a
          >
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: "navbarComp",
  props: {
    active: {
      type: String,
      default: "home", // Default active page is 'home'
    },
  },
  methods: {
    getClassName(link) {
      return `navbar__link ${this.active === link ? "active" : ""}`;
    },
    navigate(page) {
      this.$emit("navigate", page); // Emit the selected page
    },
  },
};
</script>

<style scoped>
.navbar {
  background: palegoldenrod;
  color: #000;
  padding: 1rem 0;
}

.navbar__wrap {
  list-style: none;
  display: flex;
  justify-content: flex-end;
}

.navbar__link {
  text-decoration: none;
  color: currentColor;
  padding: 0.3rem 2rem;
}

.navbar__link.active {
  font-weight: bold;
  color: red;
}
</style>
