<template>
  <div class="course__listing">
    <div v-show="loading" class="not-found">loading...</div>

    <div class="course__wrap" v-if="!loading && allCourses?.length">
      <courseItem
        v-for="(course, i) in allCourses"
        :key="course._id"
        :data="course"
        :path="images[i]"
      />
    </div>

    <div v-else-if="!loading" class="not-found">Your Courses are empty</div>
  </div>
</template>

<script>
import courseItem from "./courseItem.vue";

export default {
  name: "courseComp",
  data() {
    return {
      loading: true,
      allCourses: this.courses,
      images: [],
    };
  },
  props: {
    courses: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    courseItem,
  },
  methods: {
    async loadImages() {
      for (let i = 1; i <= 21; i++) {
        try {
          let image;
          if (i <= 5) image = await import(`../assets/images/image-${i}.webp`);

          if (i > 5 && 1 <= 21)
            image = await import(`../assets/images/image-${i}.jpg`);

          if (i > 21) image = await import("../assets/logo.png");

          this.images.push(image.default);
        } catch (error) {
          console.error(`Error loading image-${i}.webp:`, error);
        }
      }
    },
    getCourses() {
      this.loading = true;

      fetch("https://api-bookshop-com.onrender.com/v1/courses")
        .then(async (resp) => {
          const courses = await resp.json();
          this.allCourses = courses.data;
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
  },
  async mounted() {
    this.getCourses();
    await this.loadImages();
  },
};
</script>
