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
      default: "home",
    },
  },
  methods: {
    getClassName(link) {
      return `navbar__link ${this.active === link ? "active" : ""}`;
    },
    navigate(page) {
      this.$emit("navigate", page);
    },
  },
};
</script>

<style scoped>
.navbar {
  background: #333333;
  padding: 2rem 0;
  color: #fff;
}

.navbar__wrap {
  list-style: none;
  display: flex;
  justify-content: center;
}

.navbar__link {
  text-decoration: none;
  color: currentColor;
  padding: 0.3rem 2rem;
}

.navbar__link.active {
  font-weight: bold;
  text-transform: uppercase;
}
</style>
