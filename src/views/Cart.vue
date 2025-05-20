<template>
    <div class="cart">
        <Navbar/>
        <h2>Ini keranjang</h2>
    </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';

export default {
  name: 'CartView',
  components: {
    Navbar,
  },
};
</script>

<style></style>
