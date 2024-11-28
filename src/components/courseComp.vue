<template>
  <div class="course__listing">
    <div v-show="loading" class="not-found">loading...</div>
    <div v-show="!loading && !courses?.length" class="not-found">
      Your Courses are empty
    </div>
    <courseItem
      v-show="courses?.length"
      v-for="course in courses"
      :key="course._id"
      :data="course"
    />
  </div>
</template>

<script>
import courseItem from "./courseItem.vue";

export default {
  name: "courseComp",
  data() {
    return {
      loading: true,
      courses: [],
    };
  },
  components: {
    courseItem,
  },
  methods: {
    getCourses() {
      fetch("https://api-bookshop-com.onrender.com/v1/courses")
        .then(async (resp) => {
          this.loading = true;
          const courses = await resp.json();
          this.courses = courses.data;
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        })
        .finally(() => (this.loading = false));
    },

    addToCart(itemId) {
      fetch(`https://api-bookshop-com.onrender.com/v1/courses/${itemId}`)
        .then((resp) => {
          this.loading = true;
          const courses = resp.json();
          this.courses = courses.data;
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
  },
  mounted() {
    this.getCourses();
  },
};
</script>
