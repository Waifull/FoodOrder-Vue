<template>
  <div class="foods">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col" style="font-size: 34px">Daftar <strong>Makanan</strong></div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input v-model="search" type="text" class="form-control" placeholder="Cari Makanan Kesukaan Anda..." aria-label="Search" aria-describedby="basic-addon1" @keyup="searchFood" />
            <span class="input-group-text" id="basic-addon1">
              <b-icon-search></b-icon-search>
            </span>
          </div>
        </div>
      </div>

      <div class="row mb-4">
        <div class="col-md-3 mt-4" v-for="product in products" :key="product.id">
          <CardProduct :product="product" />
        </div>
        <div v-if="products.length === 0 && search !== ''" class="col text-center mt-4">Menu tidak dapat ditemukan ^_^</div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from '@/components/Navbar.vue';
import CardProduct from '@/components/CardProduct.vue';
import axios from 'axios';

export default {
  name: 'FoodsView',
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: '',
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get('http://localhost:3000/products?q=' + this.search)
        .then((response) => this.setProducts(response.data))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get('http://localhost:3000/products')
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
<style scoped></style>
