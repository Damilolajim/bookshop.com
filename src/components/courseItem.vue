<template>
  <div class="courseItem">
    <div class="couseItem__imgWrap">
      <img src="../assets/logo.png" alt="this is meant to be an image" />
    </div>
    <div class="courseItem__main">
      <ul class="courseItem__listing">
        <li class="courseItem__listItem">
          <span class="bold">Subject:</span> {{ data.subject }}
        </li>
        <li class="courseItem__listItem">
          <sapn class="bold">Location:</sapn> {{ data.location }}
        </li>
        <li class="courseItem__listItem">
          <span class="bold">Price:</span> {{ data.price }}
        </li>
        <li class="courseItem__listItem">
          <span class="bold">Spaces:</span> {{ data.spaces }}
        </li>
      </ul>
      <buttonItem text="Add to Cart" className="btn__primary" link="#" />
    </div>
  </div>
</template>

<script>
import buttonItem from "./buttonItem.vue";

export default {
  name: "courseItem",
  props: {
    data: {
      subject: String,
      location: String,
      price: Number,
      spaces: Number,
    },
  },
  components: {
    buttonItem,
  },
};
</script>

<style scoped>
.courseItem {
  padding: 2rem;
  border: 1px solid #1a1a1a;
  display: inline-block;
  border-radius: 0.8rem;
}

.couseItem__imgWrap {
  height: 8rem;
}

.couseItem__imgWrap img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

.courseItem__listing {
  list-style: none;
  margin: 0.5rem 0;
}
.courseItem__listItem {
  padding: 0.3rem 0;
  font-size: 1.8rem;
}

.bold {
  font-weight: 600;
}
</style>
