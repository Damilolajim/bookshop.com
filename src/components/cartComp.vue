<template>
  <div class="course__listing">
    <div v-show="loading" class="not-found">loading...</div>

    <div v-show="!loading && !courses?.length" class="not-found">
      Your Cart is empty
    </div>

    <div class="course__wrap" v-if="!loading && courses?.length">
      <courseItem
        v-for="(courseItem, i) in courses"
        :_carts="carts"
        :key="courseItem._id"
        :data="courseItem"
        :path="images[i]"
        :cartComponent="isCartComponent"
        @update-cart="getCarts"
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
      carts: [],
      images: [],
      courses: [],
      loading: true,
      isCartComponent: true,
    };
  },
  components: {
    courseItem,
  },
  methods: {
    async getCarts(loading = true) {
      this.loading = loading;

      fetch("https://api-bookshop-com.onrender.com/v1/carts")
        .then(async (resp) => {
          const carts = await resp.json();
          this.carts = carts;
          this.courses = carts.data.map(({ course }) => course);
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
    async loadImages() {
      for (let i = 1; i <= 21; i++) {
        try {
          let image;
          if (i <= 5) image = await import(`../assets/images/image-${i}.webp`);

          if (i >= 6 && 1 <= 21)
            image = await import(`../assets/images/image-${i}.jpg`);

          if (i > 21) image = await import("../assets/logo.png");

          this.images.push(image.default);
        } catch (error) {
          console.error(`Error loading image-${i}.webp:`, error);
        }
      }
    },
  },
  async mounted() {
    await this.getCarts();
    await this.loadImages();
  },
};
</script>
