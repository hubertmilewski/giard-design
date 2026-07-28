<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'
import Navbar from '@/components/layout/Navbar.vue'
import Intro from '@/components/sections/Intro.vue'
import AboutCompany from '@/components/sections/AboutCompany.vue'
import Ofert from '@/components/sections/Ofert.vue'
import Gallery from './components/sections/Gallery.vue'
import InstagramContact from '@/components/sections/InstagramContact.vue'
import Footer from '@/components/layout/Footer.vue'

let observer: IntersectionObserver | null = null

onMounted(() => {
  const elements = document.querySelectorAll('.fade-on-scroll')

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        const rect = entry.boundingClientRect

        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible')
        } else if (rect.top > 0) {
          entry.target.classList.remove('is-visible')
        }
      })
    },
    {
      threshold: 0.05,
      rootMargin: '0px 0px -20px 0px',
    }
  )

  elements.forEach((el) => observer?.observe(el))
})

onUnmounted(() => {
  if (observer) {
    observer.disconnect()
  }
})
</script>

<template>
  <div class="animate-fade-in">
    <Navbar />
    <Intro class="pt-18" />
    <Ofert />
    <AboutCompany />
    <Gallery />
    <InstagramContact />
    <Footer />
  </div>
</template>

<style scoped>
.animate-fade-in {
  animation: fadeInPage 1s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes fadeInPage {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
