<template>
  <div class="course__listing">
    <div v-show="loading" class="not-found">loading...</div>

    <div class="course__wrap" v-if="!loading && allCourses?.length">
      <courseItem
        v-for="course in allCourses"
        :key="course._id"
        :data="course"
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
  mounted() {
    this.getCourses();
    console.log(this.allCourses);
  },
};
</script>
