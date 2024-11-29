<template>
  <div class="filterItem">
    <h2>Sort by</h2>
    <ul class="filterItem__listing">
      <li class="filterItem__listItem" @click="setField('')">
        <input type="radio" name="sort-field" />
        <span>All</span>
      </li>
      <li class="filterItem__listItem" @click="setField('subject')">
        <input type="radio" name="sort-field" value="school" />
        <span>Subject</span>
      </li>
      <li class="filterItem__listItem" @click="setField('location')">
        <input type="radio" name="sort-field" value="location" />
        <span>Location</span>
      </li>
      <li class="filterItem__listItem" @click="setField('price')">
        <input type="radio" name="sort-field" value="price" />
        <span>Price</span>
      </li>
      <li class="filterItem__listItem" @click="setField('spaces')">
        <input type="radio" name="sort-field" value="spaces" />
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
    setField(field) {
      this.localField = field;
      this.sortCourses();
    },
    setOrder(order) {
      this.localOrder = order;
      this.sortCourses();
    },
    sortCourses() {
      this.$emit("sortCourses", {
        loading: false,
        sort: this.localField,
        order: this.localOrder,
      });
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
