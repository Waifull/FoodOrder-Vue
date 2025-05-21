<template>
  <div class="cart">
    <Navbar :updateCart="keranjangs" />
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
              <li class="breadcrumb-item active" aria-current="page">Keranjang</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">No</th>
                  <th scope="col">Menu Makanan</th>
                  <th scope="col">Nama</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img :src="require(`../assets/images/${keranjang.products.gambar}`)" alt="" class="img-fluid shadow" width="200" />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : '-' }}
                  </td>
                  <td>
                    {{ keranjang.jumlah_pemesanan }}
                  </td>
                  <td align="right">Rp {{ formatRupiah(keranjang.products.harga) }}</td>
                  <td align="right">
                    <strong>Rp {{ formatRupiah(keranjang.products.harga * keranjang.jumlah_pemesanan) }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash @click="deleteCart(keranjang.id)"></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga:</strong>
                  </td>
                  <td align="right">
                    <strong>Rp {{ formatRupiah(totalHarga) }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- Form Checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form action="" class="mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama :</label>
              <input type="text" class="form-control" v-model="pesanan.nama" />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja :</label>
              <input type="text" class="form-control" v-model="pesanan.noMeja" />
            </div>

            <button @click="checkout" type="submit" class="btn btn-success float-right"><b-icon-cart></b-icon-cart> Pesan</button>
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
  name: 'CartView',
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesanan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    formatRupiah(angka) {
      if (!angka) return '0';
      return angka.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.');
    },
    deleteCart(id) {
      axios
        .delete('http://localhost:3000/keranjangs/' + id)
        .then(() => {
          this.$toast.error('Berhasil Hapus Pesanan', {
            type: 'error',
            position: 'top-right',
            duration: 3000,
            dismissible: true,
          });

          // Update Cart Data
          axios
            .get('http://localhost:3000/keranjangs')
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    checkout() {
      if (this.pesanan.nama && this.pesanan.noMeja) {
        this.pesanan.keranjangs = this.keranjangs;
        axios
          .post('http://localhost:3000/pesanans', this.pesanan)
          .then(() => {
            // Delete All Item on Cart
            this.keranjangs.map(function (item) {
              return axios.delete('http://localhost:3000/keranjangs/' + item.id).catch((error) => console.log(error));
            });
            this.$router.push({ path: '/order-success' });
            this.$toast.success('Berhasil Membuat Pesanan', {
              type: 'success',
              position: 'top-right',
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error('Nama dan Nomor Meja Harus diisi', {
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
      .get('http://localhost:3000/keranjangs')
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce((total, item) => {
        return total + item.products.harga * item.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style></style>
