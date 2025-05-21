<template>
  <div class="food-detail">
    <Navbar />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Food Detail</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col-md-6">
          <img :src="require(`../assets/images/${product.gambar}`)" alt="" class="img-fluid shadow" />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <p class="card-text">{{ product.deskripsi }}</p>
          <h4>
            Harga: <strong>Rp {{ formatRupiah(product.harga) }}</strong>
          </h4>
          <form action="" class="mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesanan</label>
              <input type="number" class="form-control" v-model="pesanan.jumlah_pemesanan" />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea name="" id="" placeholder="Keterangan tambahan (opsional)" class="form-control" v-model="pesanan.keterangan"></textarea>
            </div>

            <button @click="pemesanan" type="submit" class="btn btn-success"><b-icon-cart></b-icon-cart> Pesan</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';

export default {
  name: 'FoodDetailView',
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesanan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    formatRupiah(angka) {
      if (!angka) return '0';
      return angka.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.');
    },
    pemesanan() {
      if (this.pesanan.jumlah_pemesanan) {
        this.pesanan.products = this.product;
        axios
          .post('http://localhost:3000/keranjangs', this.pesanan)
          .then(() => {
            this.$router.push({ path: '/cart' });
            this.$toast.success('Berhasil Masuk Keranjang', {
              type: 'success',
              position: 'top-right',
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error('Jumlah Pesanan Harus diisi', {
          type: 'error',
          position: 'top-right',
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get('http://localhost:3000/products/' + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style></style>
