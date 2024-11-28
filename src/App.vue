<template>
  <div class="body">
    <bannerComp title="Bookshop.com" />
    <nav class="navbar">
      <ul class="navbar__wrap">
        <li class="navbar__item">
          <a href="/" class="navbar__link">Home</a>
        </li>
        <li class="navbar__item">
          <a href="/" class="navbar__link">Home</a>
        </li>
      </ul>
    </nav>
    <div class="courses">
      <div class="course__filter">
        <filterItem />
      </div>
      <div class="course__listing">
        <courseItem
          v-for="course in courses"
          :key="course._id"
          :data="course"
        />
      </div>
    </div>
  </div>
</template>

<script>
import courseItem from "./components/courseItem.vue";
import filterItem from "./components/filterItem.vue";
import bannerComp from "./components/bannerComp.vue";

export default {
  name: "App",
  data() {
    return {
      courses: [
        {
          _id: "",
          subject: "",
          location: "",
          price: 0,
          spaces: 0,
        },
      ],
    };
  },
  components: {
    courseItem,
    filterItem,
    bannerComp,
  },
  methods: {
    async getCourses() {
      try {
        const resp = await fetch(
          "https://api-bookshop-com.onrender.com/v1/courses"
        );

        const courses = await resp.json();
        console.log(courses);
        this.courses = courses.data;
      } catch (err) {
        console.error("Error fetching courses:", err);
      }
    },
  },
  mounted() {
    this.getCourses();
  },
};
</script>

<style>
*,
*::after,
*::before {
  padding: 0;
  margin: 0;
  box-sizing: inherit;
}

html {
  font-size: 62.5%;
}

body {
  line-height: 1.5;
  font-size: 1.6rem;
  color: #1a1a1a;
  box-sizing: border-box;
}

.courses {
  display: flex;
  align-items: flex-start;
}

.courses > * {
  flex: 1;
}

.courses .course__filter {
  flex: 0 0 30%;
}

.course__listing {
  display: flex;
  gap: 2rem;
  flex-wrap: wrap;
}

.course__listing > * {
  flex: 0 0 calc((100% - (2rem * 2)) / 3);
}
</style>
