<template>
  <div class="order-success">
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="../assets/images/menunggu.png" width="300" alt="">
          <h2><strong>Sukses!!</strong></h2>
          <h4>Pesanan Anda Segera Diproses <br> Selamat Menunggu</h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';

export default {
  name: 'OrderSuccessView',
  components: {
    Navbar,
  }
};
</script>

<style></style>
