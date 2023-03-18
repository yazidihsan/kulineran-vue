<template>
  <div class="card shadow card-product" style="width: 18rem">
    <img
      :src="'assets/images/' + produk.gambar"
      alt="image"
      class="card-img-top"
    />
    <div class="card-body">
      <h5 class="card-title">{{ produk.nama }}</h5>
      <p class="card-text">Harga : Rp. {{ produk.harga }}</p>
      <router-link :to="'/foods/' + produk.id" class="btn btn-success"
        ><b-icon-cart></b-icon-cart> Pesan</router-link
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProductItem",
  props: ["produk"],
};
</script>
