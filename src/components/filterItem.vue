<template>
  <div class="filterItem">
    <h2>Sort by</h2>
    <ul class="filterItem__listing">
      <li class="filterItem__listItem" @click="setField('all')">
        <input type="radio" name="sort-field" value="" />
        <span>All</span>
      </li>
      <li class="filterItem__listItem" @click="setField('school')">
        <input type="radio" name="sort-field" value="school" />
        <span>School</span>
      </li>
      <li class="filterItem__listItem" @click="setField('location')">
        <input type="radio" name="sort-field" value="location" />
        <span>Location</span>
      </li>
      <li class="filterItem__listItem" @click="setField('price')">
        <input type="radio" name="sort-field" value="price" />
        <span>Price</span>
      </li>
      <li class="filterItem__listItem" @click="setField('availability')">
        <input type="radio" name="sort-field" value="availability" />
        <span>Availability</span>
      </li>
    </ul>
    <ul class="filterItem__listing">
      <li class="filterItem__listItem" @click="setOrder('asc')">
        <input type="radio" name="sort-order" value="asc" />
        <span>Ascending</span>
      </li>
      <li class="filterItem__listItem" @click="setOrder('desc')">
        <input type="radio" name="sort-order" value="desc" />
        <span>Descending</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "filterItem",
  data() {
    return {
      loading: true,
      localField: this.field || "",
      localOrder: this.order,
    };
  },
  props: {
    field: {
      type: String,
      default: "",
    },
    order: {
      type: String,
      default: "asc",
    },
  },
  methods: {
    sortCourses() {
      this.loading = true;

      fetch(
        `https://api-bookshop-com.onrender.com/v1/courses?sort=${this.localField}&order=${this.localOrder}`
      )
        .then(async (resp) => {
          const courses = await resp.json();
          this.$emit("update-courses", courses.data);
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        });
    },

    setField(field) {
      if (field === "all") {
        this.localField = this.localOrder = "";
        this.sortCourses();
        return;
      }

      this.localField = field;
      this.sortCourses();
    },

    setOrder(order) {
      this.localOrder = order;
      this.sortCourses();
    },
  },
};
</script>

<style scoped>
.filterItem {
  padding: 2rem;
}

.filterItem__listing {
  list-style: none;
  padding: 1rem 2rem;
}

.filterItem__listing:not(:last-child) {
  margin-bottom: 2rem;
}

.filterItem__listItem {
  display: flex;
  gap: 1rem;
  align-items: center;
}
</style>
