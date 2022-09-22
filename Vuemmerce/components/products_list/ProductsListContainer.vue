<template>
  <div class="m-5">
    <table class="table-fixed bg-grey_light">
      <tr>
        <th>Item</th>
        <th>Price/kg</th>
        <th>Quantity</th>
        <th>Add/Remove</th>
      </tr>
    </table>

    <div class="grid grid-cols-1 md:grid-cols-1 lg:grid-cols-1 font-semibold">
      <div v-for="product in products" :key="product.id">
        <Products :detail="false" :product="product" />
      </div>
    </div>
    <div class="text-center" v-if="products.length === 0">
      <h4 class="text-2xl">{{ noProductLabel }}</h4>
    </div>
  </div>
</template>

<script>
import Products from "../Products";
import { getByTitle } from "@/assets/filters";

export default {
  name: "productsList",

  components: {
    Products
  },

  data() {
    return {
      id: "",
      noProductLabel: "No product found"
    };
  },

  computed: {
    products() {
      const {
        products,
        userInfo: { hasSearched }
      } = this.$store.state;

      if (hasSearched) {
        return this.getProductByTitle();
      } else {
        return products;
      }
    }
  },

  methods: {
    getProductByTitle() {
      const {
        products,
        userInfo: { productTitleSearched }
      } = this.$store.state;

      return getByTitle(products, productTitleSearched);
    }
  }
};
</script>

<style scoped>
table {
  font-family: "roboto", serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px 8px;
}

/* tr:nth-child(even) {
  background-color: #dddddd;
} */
</style>
