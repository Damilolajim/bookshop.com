<template>
  <div class="body">
    <bannerComp title="Bookshop.com" />
    <navbarComp :active="currentView" @navigate="changeView" />
    <div class="container">
      <div class="courses">
        <div class="course__filter">
          <filterItem @sortCourses="sortCourses" />
        </div>

        <courseComp
          v-if="currentView === 'home'"
          :updatedCourse="updatedCourses"
        />

        <cartComp v-if="currentView === 'cart'" />
      </div>
    </div>
  </div>
</template>

<script>
import filterItem from "./components/filterItem.vue";
import bannerComp from "./components/bannerComp.vue";
import navbarComp from "./components/navbarComp.vue";
import cartComp from "./components/cartComp.vue";
import courseComp from "./components/courseComp.vue";

export default {
  name: "App",
  data() {
    return {
      currentView: "home",
      updatedCourses: undefined,
    };
  },
  components: {
    filterItem,
    navbarComp,
    cartComp,
    bannerComp,
    courseComp,
  },
  methods: {
    sortCourses({ loading = false, sort = undefined, order = "asc" } = {}) {
      this.loading = loading;

      fetch(
        `https://api-bookshop-com.onrender.com/v1/courses?sort=${sort}&order=${order}`
      )
        .then(async (resp) => {
          const courses = await resp.json();
          this.updatedCourses = courses.data;
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
    changeView(view) {
      this.currentView = view;
    },
    updateCourses(newCourses) {
      this.updatedCourses = newCourses;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Epilogue:ital,wght@0,100..900;1,100..900&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Prompt:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

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
  font-family: "Prompt", serif;
  font-style: normal;
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
  gap: 3rem;
}

.courses > * {
  flex: 1 0 75%;
}

.courses .course__filter {
  flex: 0 0 25%;
}

.course__wrap {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
  padding: 1rem;
  align-items: flex-start;
}

.course__listing .not-found {
  min-height: 50rem;
  font-size: 4rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  background: #4d4d4d;
  color: #fff;
  flex: 1 0 100%;
}

.course__wrap > * {
  flex: 0 0 calc((100% - (1.5rem * 3)) / 4);
}
</style>
