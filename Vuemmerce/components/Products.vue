<template>
  <div>
    <table
      class="table-fixed table is-bordered is-striped is-narrow is-hoverable is-fullwidth"
    >
      <tbody>
        <tr class="even:bg-amber-100 odd:bg-blue-100">
          <td>
            <nuxt-link
              :to="{
                name: 'product_detail-id',
                params: {
                  id: product.id,
                  title: product.title,
                  price: product.price,
                  isAddedBtn: product.isAddedBtn
                }
              }"
            >
            </nuxt-link>

            <div>
              <div>
                <nuxt-link
                  :to="{
                    name: 'product_detail-id',
                    params: {
                      id: product.id,
                      title: product.title,
                      price: product.price,
                      rating: product.ratings,
                      reviews: product.reviews,
                      isAddedBtn: product.isAddedBtn
                    }
                  }"
                >
                  <span> {{ product.title }}</span>
                </nuxt-link>
              </div>
            </div>
          </td>
          <td>
            <p>
              <strong>&#8377; {{ product.price }}</strong>
            </p>
          </td>
          <td>
            <select @change="onSelectQuantity(product.id)" v-model="selected">
              <option
                v-bind:key="quantity"
                v-for="quantity in quantityArray"
                :value="quantity"
              >
                {{ quantity }}
              </option>
            </select>
          </td>

          <!-- add to cart starts . -->

          <td>
            <div>
              <!-- ----- check -->
              <input
                v-if="!product.isAddedToCart"
                type="checkbox"
                :checked="value"
                @change="addToCart(product.id)"
              />

              <input
                v-if="product.isAddedToCart"
                type="checkbox"
                :checked="value"
                @change="removeFromCart(product.id, false)"
              />

              <!-- -----  check -->

              <!-- <button
                class="rounded-xl p-3 bg-blue text-white"
                v-if="!product.isAddedToCart"
                @click="addToCart(product.id)"
              >
                {{ addToCartLabel }}
              </button>
              <button
                class="rounded-xl p-3"
                v-if="product.isAddedToCart"
                @click="removeFromCart(product.id, false)"
              >
                {{ removeFromCartLabel }}
              </button> -->
            </div>
          </td>

          <!-- add to cart ends  -->
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "products",
  props: ["product"],
  // props: ["product", "detail"],

  data() {
    return {
      addToCartLabel: "Add",
      viewDetailsLabel: "Details",
      removeFromCartLabel: "Remove",
      selected: 1,
      quantityArray: []
    };
  },

  mounted() {
    for (let i = 1; i <= 5; i += 0.5) {
      this.quantityArray.push(i);
    }

    if (this.$props.product.quantity > 1) {
      this.selected = this.$props.product.quantity;
    }
  },

  computed: {
    isUserLogged() {
      return this.$store.getters.isUserLoggedIn;
    }
  },

  methods: {
    addToCart(id) {
      let data = {
        id: id,
        status: true
      };
      this.$store.commit("addToCart", id);
      this.$store.commit("setAddedBtn", data);
    },
    removeFromCart(id) {
      let data = {
        id: id,
        status: false
      };
      this.$store.commit("removeFromCart", id);
      this.$store.commit("setAddedBtn", data);
    },
    onSelectQuantity(id) {
      let data = {
        id: id,
        quantity: this.selected
      };
      this.$store.commit("quantity", data);
    }
  }
};
</script>

<style>
table {
  font-family: roboto, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

table td:nth-child(1) {
  font-family: "Noto Serif Kannada", serif;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 2px 4px;
}

/* .mytable tbody > tr:nth-child(even) {
  background-color: #dddddd;
} */

/* .mytable tbody > tr:nth-child(odd) {
  background-color: rgb(133, 8, 8);
} */
</style>
