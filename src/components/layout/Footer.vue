<script setup lang="ts">
import Buttons from '@/components/ui/Buttons.vue'
import logo from '@/assets/icons/logo.svg'
import adRespectLogo from '@/assets/icons/footer/adrespectLogo.webp'

const footerLinks = [
  { label: 'Kontakt', href: '/kontakt' },
  { label: 'Instagram', href: '#' },
  { label: 'Facebook', href: '#' },
  { label: 'LinkedIn', href: '#' },
]

const contactDetails = [
  { label: '000-000-000', href: 'tel:000000000' },
  { label: 'giarddesign@kontakt.pl', href: 'mailto:giarddesign@kontakt.pl' },
]
</script>

<template>
  <footer class="w-full bg-bg-dark text-base">
    <div class="container-custom mx-auto pt-10 pb-8 lg:pt-25 lg:pb-25">
      <div
        class="flex flex-col lg:flex-row items-start lg:items-center justify-between gap-8 lg:gap-0"
      >
        <a href="/" class="inline-flex items-center">
          <img :src="logo" alt="GiardDesign" class="h-6 w-auto filter brightness-0 invert" />
        </a>

        <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 sm:gap-6">
          <p class="text-[16px] text-base">Daj znać, co możemy dla Ciebie zrobić!</p>
          <Buttons primaryHref="/kontakt">
            <template #primary>Skontaktuj się z nami</template>
          </Buttons>
        </div>
      </div>

      <hr class="my-8 lg:my-15.5" />

      <div
        class="flex flex-col lg:flex-row items-start lg:items-center justify-between gap-10 lg:gap-12 text-[14px] text-base"
      >
        <ul class="flex flex-col sm:flex-row items-start sm:items-center gap-4 lg:gap-12">
          <li v-for="link in footerLinks" :key="link.label">
            <a :href="link.href" class="hover:text-base/90 transition-colors">
              {{ link.label }}
            </a>
          </li>
        </ul>

        <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 lg:gap-12">
          <a
            v-for="contact in contactDetails"
            :key="contact.label"
            :href="contact.href"
            class="hover:text-base/90 transition-colors"
          >
            {{ contact.label }}
          </a>
        </div>
      </div>

      <div
        class="mt-12 lg:mt-30 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-6 lg:gap-0 text-[16px] text-base"
      >
        <div>Prawa zastrzeżone © 2022</div>

        <div class="flex items-center gap-4">
          <span>made by</span>
          <img :src="adRespectLogo" alt="adRespect" class="h-5 w-auto" />
        </div>
      </div>
    </div>
  </footer>
</template>
