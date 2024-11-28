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
          <span class="bold">Location:</span> {{ data.location }}
        </li>
        <div class="flex">
          <li class="courseItem__listItem">
            <span class="bold">Price:</span> {{ data.price }}
          </li>
          <li class="courseItem__listItem">
            <span class="bold">Spaces:</span> {{ data.spaces }}
          </li>
        </div>
      </ul>
    </div>
    <buttonItem text="Add to Cart" className="btn__primary" link="#" />
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
  border: 1px solid #1a1a1a89;
  display: inline-block;
  border-radius: 0.8rem;
  overflow: hidden;
}

.courseItem__main {
  padding: 0.7rem 1.4rem;
}

.couseItem__imgWrap {
  height: 15rem;
}

.couseItem__imgWrap img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

.courseItem__listing {
  list-style: none;
}
.courseItem__listItem {
  padding: 0.18rem 0;
  display: flex;
  justify-content: space-between;
}

.bold {
  font-weight: 600;
}

.flex {
  display: flex;
  gap: 1.8rem;
  justify-content: space-between;
}

.flex > * {
  flex: 0 0 calc((100% - 1.8rem) / 2);
}
</style>
