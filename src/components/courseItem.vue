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
          <span class="bold">Price:</span> {{ data.price }}
        </li>
        <li class="courseItem__listItem">
          <span class="bold">Spaces:</span> {{ data.spaces }}
        </li>
      </ul>
    </div>
    <buttonItem
      link="#"
      :text="getCartItem(data._id) ? 'Remove' : 'Add to CArt'"
      className="btn__primary"
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
  props: {
    path: String,
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
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        })
        .finally(() => (this.loading = false));
    },

    getCartItem(itemId) {
      this.loading = true;

      fetch(`https://api-bookshop-com.onrender.com/v1/carts/${itemId}`)
        .then(async (resp) => {
          const cartResp = await resp.json();
          return cartResp.data.successful;
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
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
