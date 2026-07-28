<script setup lang="ts">
import Navbar from '@/components/layout/Navbar.vue'
import Intro from '@/components/sections/Intro.vue'
import AboutCompany from '@/components/sections/AboutCompany.vue'
import Ofert from '@/components/sections/Ofert.vue'
import Gallery from './components/sections/Gallery.vue'
import InstagramContact from '@/components/sections/InstagramContact.vue'
import Footer from '@/components/layout/Footer.vue'
</script>

<template>
  <Navbar />
  <Intro class="pt-18" />
  <Ofert />
  <AboutCompany />
  <Gallery />
  <InstagramContact />
  <Footer />
</template>

<style scoped></style>
