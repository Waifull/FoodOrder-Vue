<template>
  <div class="card shadow card-product">
    <img :src="require(`../assets/images/${product.gambar}`)" class="card-img-top" alt="..." />
    <div class="card-body">
      <h5 class="card-title">{{ product.nama }}</h5>
      <p class="card-text">Harga: Rp {{ formatRupiah(product.harga) }}</p>
      <router-link class="btn btn-success" :to="'/foods/' + product.id"><b-icon-cart></b-icon-cart> Pesan</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardProductComponent',
  props: ['product'],
  methods: {
    formatRupiah(angka) {
      if (!angka) return '0';
      return angka.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.');
    },
  },
};
</script>

<style></style>
