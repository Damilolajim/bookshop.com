<template>
  <div class="body">
    <bannerComp title="Bookshop.com" />
    <navbarComp :active="currentView" @navigate="changeView" />
    <div class="container">
      <div class="courses">
        <div class="course__filter">
          <filterItem />
        </div>
        <courseComp v-if="currentView === 'home'" />
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
    changeView(view) {
      this.currentView = view;
    },
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
  flex: 1 0 75%;
}

.courses .course__filter {
  flex: 0 0 25%;
}

.course__listing {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
}

.course__listing .not-found {
  min-height: 50rem;
  font-size: 4rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  background: palegoldenrod;
  flex: 1 0 100%;
}

.course__listing > * {
  flex: 0 0 calc((100% - (1.5rem * 3)) / 4);
}
</style>
