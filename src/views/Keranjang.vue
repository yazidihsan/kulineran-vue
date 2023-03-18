<template>
  <div class="keranjang">
    <NavBar :updateKeranjang="keranjangs" />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2><strong>Keranjang</strong></h2>
        </div>
      </div>
      <div class="table-responsive mt-4">
        <table class="table">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Foto</th>
              <th scope="col">Makanan</th>
              <th scope="col">Keterangan</th>
              <th scope="col">Jumlah</th>
              <th scope="col">Harga</th>
              <th scope="col">Total Harga</th>
              <th scope="col">Hapus</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
              <th scope="row">{{ index + 1 }}</th>
              <td>
                <img
                  :src="'assets/images/' + keranjang.products.gambar"
                  alt="gambar foto"
                  class="img-fluid shadow"
                  width="250"
                />
              </td>
              <td>
                <strong>{{ keranjang.products.nama }}</strong>
              </td>
              <td>
                {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
              </td>
              <td>{{ keranjang.jumlah_pemesanan }}</td>
              <td align="right">Rp. {{ keranjang.products.harga }}</td>
              <td align="right">
                Rp. {{ keranjang.jumlah_pemesanan * keranjang.products.harga }}
              </td>
              <td align="center" class="text-danger">
                <b-icon-trash
                  @click="hapusKeranjang(keranjang.id)"
                ></b-icon-trash>
              </td>
            </tr>
            <tr>
              <td colspan="6" align="right">
                <strong>Total Harga :</strong>
                Rp. {{ totalHarga }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama : </label>
              <input type="text" class="form-control" v-model="pesanan.nama" />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja : </label>
              <input
                type="text"
                class="form-control"
                v-model="pesanan.noMeja"
              />
            </div>
            <button
              type="submit"
              class="btn btn-success float-right"
              @click="checkout"
            >
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
import axios from "axios";

export default {
  name: "KeranjangItem",
  components: {
    NavBar,
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
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.error("Sukses hapus keranjang", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });

          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjangs(response.data))
            .catch((err) => console.log(err));
        })
        .catch((err) => console.log(err));
    },
    checkout() {
      if (this.pesanan.nama && this.pesanan.noMeja) {
        this.pesanan.keranjangs = this.keranjangs;
        axios
          .post("http://localhost:3000/pesanans", this.pesanan)
          .then(() => {
            this.keranjangs.map((item) => {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)

                .catch((err) => console.log(err));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Pemesanan Berhasil", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Nama Pemesan atau Nomor Meja tidak diisi", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch((err) => console.log(err));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce((prev, current) => {
        return prev + current.products.harga * current.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>
