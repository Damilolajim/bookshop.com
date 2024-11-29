<template>
  <div class="course__listing">
    <div v-show="loading" class="not-found">loading...</div>
    <div v-show="!loading && !carts?.length" class="not-found">
      Your Cart is empty
    </div>

    <div class="course__wrap">
      <courseItem
        v-show="carts?.length"
        v-for="cart in carts"
        :key="cart._id"
        :data="cart"
      />
    </div>
  </div>
</template>

<script>
import courseItem from "./courseItem.vue";

export default {
  name: "courseComp",
  data() {
    return {
      loading: true,
      carts: [],
    };
  },
  components: {
    courseItem,
  },
  methods: {
    getCarts() {
      this.loading = true;

      fetch("https://api-bookshop-com.onrender.com/v1/carts")
        .then(async (resp) => {
          const carts = await resp.json();
          this.carts = carts.data;
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        })
        .finally(() => (this.loading = false));
    },

    removeFromCart(itemId) {
      this.loading = true;

      fetch(`https://api-bookshop-com.onrender.com/v1/courses/${itemId}`)
        .then((resp) => {
          const carts = resp.json();
          this.carts = carts.data;
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
  },
  mounted() {
    this.getCarts();
  },
};
</script>
