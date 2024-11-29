<template>
  <div class="course__listing">
    <div v-show="loading" class="not-found">
      <div v-show="loading" class="not-found">
        <loaderComp />
      </div>
    </div>

    <div v-show="checkedOut && !loading" class="not-found">
      Checkout successful
    </div>

    <div v-show="!loading && !courses?.length" class="not-found">
      Your Cart is empty
    </div>

    <div class="course__main" v-if="!loading && courses?.length">
      <div class="pad">
        <header class="checkout__header">
          <h2 class="course__heading">Cheackout</h2>
          <span class="edit"><a href="#courseListing">Edit</a></span>
        </header>
      </div>
      <div class="checkout">
        <div class="checkout__tableWrap">
          <table class="checkout__table">
            <thead class="checkout__tableHead">
              <tr class="checkout__tableRow">
                <th class="checkout__tableData head">Subject</th>
                <th class="checkout__tableData head">Location</th>
                <th class="checkout__tableData head">Price</th>
                <th class="checkout__tableData head">Quantity</th>
                <th class="checkout__tableData head">Total (&pound;)</th>
              </tr>
            </thead>
            <tbody class="checkout__tableBody">
              <tr
                v-for="(cart, i) in courses"
                :key="cart._id"
                class="checkout__tableRow"
              >
                <td class="checkout__tableData">
                  {{ cart.subject }}
                </td>
                <td class="checkout__tableData center">
                  {{ cart.location }}
                </td>
                <td class="checkout__tableData center">
                  {{ cart.price }}
                </td>
                <td class="checkout__tableData center">
                  {{ checkoutCart.data[i].quantity }}
                </td>
                <td class="checkout__tableData center">
                  {{ checkoutCart.data[i].total }}
                </td>
              </tr>
              <tr class="checkout__tableRow bold">
                <td class="checkout__tableData" colspan="4">
                  Total Cart Calue
                </td>
                <td class="checkout__tableData center">
                  {{ checkoutCart.totalCartValue }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="checkout__info">
          <form action="#">
            <div class="flex">
              <div class="form-group">
                <label for="name">Name</label>
                <input
                  type="text"
                  id="name"
                  required
                  autocomplete="off"
                  v-model="checkout.name"
                />
              </div>
              <div class="form-group">
                <label for="email">Email</label>
                <input
                  type="email"
                  id="email"
                  v-model="checkout.email"
                  required
                  autocomplete="off"
                />
              </div>
            </div>
            <div class="right-align">
              <buttonItem
                link="#"
                className="btn__primary"
                text="checkout"
                @click.prevent="checkoutCarts"
              />
            </div>
          </form>
        </div>
      </div>

      <div id="courseListing">
        <h2 class="course__heading">Course Checkout Listing</h2>
        <div class="course__wrap">
          <courseItem
            v-for="(courseItem, i) in courses"
            :_carts="carts"
            :key="courseItem._id"
            :data="courseItem"
            :path="images[i]"
            :cartComponent="isCartComponent"
            @update-cart="getCarts"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import courseItem from "./courseItem.vue";
import buttonItem from "./buttonItem.vue";
import loaderComp from "./loaderComp.vue";

export default {
  name: "courseComp",
  data() {
    return {
      carts: [],
      images: [],
      courses: [],
      checkoutCart: [],
      loading: true,
      checkedOut: false,
      checkout: {
        name: "",
        email: "",
        carts: [],
      },
      isCartComponent: true,
    };
  },
  components: {
    courseItem,
    buttonItem,
    loaderComp,
  },
  methods: {
    getCarts(loading = true) {
      this.checkedOut = false;
      this.loading = loading;

      fetch("https://api-bookshop-com.onrender.com/v1/carts")
        .then(async (resp) => {
          const carts = await resp.json();
          this.carts = carts;
          this.checkoutCart = carts;
          this.courses = carts.data.map(({ course }) => course);
        })
        .catch((err) => {
          console.error(`Error fetching courses: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
    removeFromCart(itemId) {
      this.loading = true;

      fetch(`https://api-bookshop-com.onrender.com/v1/courses/${itemId}`)
        .then((resp) => {
          const carts = resp.json();
          console.log(carts);
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        })
        .finally(() => (this.loading = false));
    },
    checkoutCarts() {
      const carts = this.checkoutCart.data.map((itm) => ({
        course_id: itm.course_id,
        quantity: itm.quantity,
      }));

      const requestBody = {
        name: this.checkout.name,
        email: this.checkout.email,
        carts,
      };

      const options = {
        method: "POST",
        body: JSON.stringify(requestBody),
        headers: {
          "Content-Type": "application/json",
        },
      };

      console.log(carts);

      fetch("https://api-bookshop-com.onrender.com/v1/carts/checkout", options)
        .then(async (resp) => {
          const response = await resp.json();
          if (response.success) this.checkedOut = response.success;
          setTimeout(() => {
            this.getCarts();
          }, 5000);
        })
        .catch((err) => {
          console.error(`Error adding course to cart: ${err}`);
        });
    },
    async loadImages() {
      for (let i = 1; i <= 21; i++) {
        try {
          let image;
          if (i <= 5) image = await import(`../assets/images/image-${i}.webp`);

          if (i >= 6 && 1 <= 21)
            image = await import(`../assets/images/image-${i}.jpg`);

          if (i > 21) image = await import("../assets/logo.png");

          this.images.push(image.default);
        } catch (error) {
          console.error(`Error loading image-${i}.webp:`, error);
        }
      }
    },
  },
  async mounted() {
    this.getCarts();
    await this.loadImages();
    console.log(this.carts);
  },
};
</script>

<style scoped>
.checkout {
  background: #efefef;
  padding: 2rem;
  margin-bottom: 5rem;
  overflow-x: scroll;
  overflow-y: hidden;
}

.checkout .btn__primary {
  width: 20%;
  margin-top: 1.6rem;
}

@media screen and (max-width: 965px) {
  .checkout .btn__primary {
    width: 30%;
  }
}

@media screen and (max-width: 555px) {
  .checkout .btn__primary {
    width: 100%;
  }
}

.right-align {
  text-align: right;
}

@media screen and (max-width: 965px) {
  .right-align {
    text-align: left;
  }
}

.pad {
  padding: 0 1rem;
}

.checkout__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: currentColor;
}

.checkout__table {
  width: 100%;
  min-width: 60rem;
  border-collapse: collapse;
}

.checkout__tableWrap {
  margin-bottom: 4rem;
  width: 100%;
  overflow-x: scroll;
  overflow-y: hidden;
}

.checkout__tableWrap::-webkit-scrollbar {
  display: none;
}

.checkout__tableHead {
  background: #333333;
  color: #fff;
}

.checkout__tableData {
  padding: 1rem;
}

.checkout__tableRow:nth-child(even) {
  background: #80808048;
}

.checkout__tableData.center {
  text-align: center;
}

.checkout__tableRow.bold {
  font-size: 1.8rem;
  font-weight: 700;
}

.flex {
  display: flex;
  justify-content: space-between;
  gap: 2rem;
}

@media screen and (max-width: 965px) {
  .flex {
    flex-direction: column;
  }
}

.form-group {
  margin-bottom: 1rem;
}

.form-group > * {
  display: block;
}

.form-group label {
  margin-bottom: 0.6rem;
  font-weight: 600;
}

.form-group input {
  padding: 1rem 1.5rem;
  width: 100%;
  border: none;
  outline: none;
  border-bottom: 2px solid transparent;
}

.form-group input:focus {
  border-bottom: 2px solid #4d4d4d;
  background: #4d4d4d60;
}

.flex > * {
  flex: 1 0 calc((100% - 2rem) / 2);
}
</style>
