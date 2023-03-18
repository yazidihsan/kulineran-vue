<template>
  <div class="pesanan-sukses">
    <NavBar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img
            src="../assets/images/menunggu.png"
            alt="gambar-menunggu"
            width="300"
          />
          <h2><strong>Sukses!!</strong></h2>
          <h4>
            Pesanan Anda segera diproses <br />
            Selamat Menunggu
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
export default {
  name: "PesananSuksesItem",
  components: {
    NavBar,
  },
};
</script>

<style></style>
