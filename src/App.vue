<template>
  <div class="bg-black/80 text-white min-h-screen">
    <Navbar />
    <Home />
    <Footer />
  </div>
</template>

<script setup>
import Navbar from "./components/Navbar.vue";
import Home from "./views/Home.vue";
import Footer from "./components/Footer.vue";
</script>
