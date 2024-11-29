<template>
  <div class="courseItem">
    <div class="couseItem__imgWrap">
      <img
        :key="Math.ceil(Math.random() * 1000)"
        :src="path"
        alt="course image"
      />
    </div>
    <div class="courseItem__main">
      <ul class="courseItem__listing">
        <li class="courseItem__listItem">
          <span class="bold">Subject:</span> {{ data.subject }}
        </li>
        <li class="courseItem__listItem">
          <span class="bold">Location:</span> {{ data.location }}
        </li>
        <li class="courseItem__listItem">
          <span class="bold">Price:</span> &pound;{{ data.price }}
        </li>
        <li class="courseItem__listItem">
          <span class="bold">Spaces:</span> {{ data.spaces }}
        </li>
      </ul>
    </div>
    <buttonItem
      link="#"
      className="btn__primary btn__block"
      :text="getCartItem(data._id) ? 'Remove' : 'Add to Cart'"
      @click.prevent="
        getCartItem(data._id) ? deleteCartItem(data._id) : addToCart(data._id)
      "
    />
  </div>
</template>

<script>
import buttonItem from "./buttonItem.vue";

export default {
  name: "courseItem",
  data() {
    return {
      loading: true,
      isCartComponent: this.cartComponent,
      carts: this._carts,
      cartIDs: [],
    };
  },
  props: {
    _carts: Object,
    path: String,
    cartComponent: Boolean,
    data: {
      _id: String,
      subject: String,
      location: String,
      price: Number,
      spaces: Number,
    },
  },
  components: {
    buttonItem,
  },
  methods: {
    addToCart(itemId) {
      this.loading = true;

      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          course_id: itemId,
          quantity: 1,
        }),
      };

      fetch("https://api-bookshop-com.onrender.com/v1/carts", options)
        .then(async (resp) => {
          const response = await resp.json();
          console.log(response);
          this.$emit("update-course", false);
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
    deleteCartItem(itemId) {
      this.loading = true;

      const options = {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
        },
      };

      fetch(`https://api-bookshop-com.onrender.com/v1/carts/${itemId}`, options)
        .then(async (resp) => {
          const response = await resp.json();
          console.log(response);
          this.$emit("update-cart", false);
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
    fetchCartItems() {
      if (!this.isCartComponent) return;
      this.cartIDs = this.carts.data.map((itm) => itm.course_id);
    },
    getCartItem(itemId) {
      return this.cartIDs.includes(itemId);
    },
  },
  mounted() {
    this.fetchCartItems();
  },
};
</script>

<style scoped>
.courseItem {
  display: inline-block;
  border-radius: 0.8rem;
  overflow: hidden;
  background: #4d4d4d;
  color: #fff;
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
  margin: 0.5rem 0;
}

.courseItem__listItem {
  padding: 0.18rem 0;
  display: flex;
  justify-content: space-between;
}

.courseItem__flex > * {
  flex: 1;
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
