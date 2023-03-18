<template>
  <div>
    <NavBar />
    <div class="container">
      <div class="row mt-4 mb-4">
        <div class="col">
          <h2>Daftar <strong>Makanan</strong></h2>
        </div>
      </div>
      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              type="text"
              class="form-control"
              placeholder="Cari makanan kesukaan Anda.."
              aria-label="Cari"
              aria-describedby="basic-addon1"
              @keyup="searchProduct"
            />
            <span class="input-group-text" id="basic-addon1"
              ><b-icon-search></b-icon-search
            ></span>
          </div>
        </div>
      </div>
      <div class="row mt-4">
        <div
          class="col-md-4 mb-2"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :produk="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
import CardProduct from "@/components/CardProducts.vue";
import axios from "axios";
export default {
  name: "FoodItem",
  components: {
    NavBar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchProduct() {
      axios
        .get("http://localhost:3000/products?q=" + this.search)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
