<template>
  <div class="body">
    <bannerComp title="Bookshop.com" />
    <navbarComp :active="currentView" @navigate="changeView" />
    <div class="container">
      <div class="courses">
        <div class="course__filter">
          <filterItem />
        </div>
        <courseComp v-if="currentView === 'home'">
          <courseItem
            v-for="course in courses"
            :key="course._id"
            :data="course"
          />
        </courseComp>
        <cartComp v-if="currentView === 'cart'"> </cartComp>
      </div>
    </div>
  </div>
</template>

<script>
import courseItem from "./components/courseItem.vue";
import filterItem from "./components/filterItem.vue";
import bannerComp from "./components/bannerComp.vue";
import navbarComp from "./components/navbarComp.vue";
import cartComp from "./components/cartComp.vue";
import courseComp from "./components/courseComp.vue";

export default {
  name: "App",
  data() {
    return {
      currentView: "home", // Default view
      courses: [],
    };
  },
  components: {
    courseItem,
    filterItem,
    navbarComp,
    cartComp,
    bannerComp,
    courseComp,
  },
  methods: {
    async getCourses() {
      try {
        const resp = await fetch(
          "https://api-bookshop-com.onrender.com/v1/courses"
        );
        const courses = await resp.json();
        this.courses = courses.data;
      } catch (err) {
        console.error("Error fetching courses:", err);
      }
    },
    changeView(view) {
      this.currentView = view;
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

.container {
  max-width: 140rem;
  margin: auto;
  padding: 0 4rem;
}

.courses {
  display: flex;
  align-items: flex-start;
  padding: 6rem 0;
}

.courses > * {
  flex: 1;
}

.courses .course__filter {
  flex: 0 0 25%;
}

.course__listing {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
}

.course__listing > * {
  flex: 0 0 calc((100% - (1.5rem * 3)) / 4);
}
</style>
