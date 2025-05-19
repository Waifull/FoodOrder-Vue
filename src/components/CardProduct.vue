<template>
  <div class="card">
    <img :src="require(`../assets/images/${product.gambar}`)" class="card-img-top" alt="..." />
    <div class="card-body">
      <h5 class="card-title">{{ product.nama }}</h5>
      <p class="card-text">{{ product.deskripsi }}</p>
      <p class="card-text">Harga : Rp {{ product.harga }}</p>
      <a href="#" class="btn btn-primary">Go somewhere</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardProductComponent',
  props: ['product'],
};
</script>

<style></style>
