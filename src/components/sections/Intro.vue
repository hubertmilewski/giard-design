<script setup lang="ts">
import ContentBlock from '@/components/ui/ContentBlock.vue'
import Buttons from '@/components/ui/Buttons.vue'

import buttonArrowDown from '@/assets/icons/buttonArrowDown.svg'
import arrowLeft from '@/assets/icons/arrowLeft.svg'
import arrowRight from '@/assets/icons/arrowRight.svg'

import Image from '@/assets/images/intro-image.webp'
import Image2 from '@/assets/images/intro-image2.webp'
import Image3 from '@/assets/images/intro-image3.webp'
</script>

<template>
  <section class="w-full bg-bg-sand overflow-hidden">
    <div class="grid grid-cols-1 lg:grid-cols-12 min-h-184.25 items-center">
      <!-- Lewa strona -->
      <div
        class="lg:col-span-6 flex flex-col justify-center py-16 lg:py-0 pl-6 sm:pl-12 lg:pl-[max(1.5rem,calc((100vw-var(--container-max,1280px))/2))]"
      >
        <ContentBlock titleClass="text-[40px] lg:text-[60px] leading-[1.1]">
          <template #title>
            Nowoczesna aranżacja <br class="hidden lg:block" />Twojego ogrodu
          </template>

          <template #description>
            Marka GiardDesign to wieloletnie doświadczenie i wysoka estetyka realizacji. Oferujemy
            kompleksowy zakres usług z indywidualnym podejściem do każdego projektu.
          </template>

          <template #actions>
            <Buttons primaryHref="/oferta" secondaryHref="/kontakt">
              <template #primary>Skontaktuj się z nami</template>
              <template #secondary>
                Zobacz nasze realizacje <img class="ml-2" :src="buttonArrowDown" alt="arrowDown" />
              </template>
            </Buttons>
          </template>
        </ContentBlock>
      </div>

      <!-- Prawa strona -->
      <div class="lg:col-span-6 self-stretch relative min-h-100 lg:min-h-0 w-full">
        <img
          :src="Image"
          alt="Zdjecie ogrodu"
          class="absolute inset-0 w-full h-full object-cover"
          fetchpriority="high"
          decoding="sync"
        />

        <!-- Slider -->
        <div class="absolute bottom-0 right-0 flex bg-white z-10">
          <button
            type="button"
            class="flex items-center justify-center w-16 h-16 sm:w-20 sm:h-20 bg-white hover:bg-neutral-100 transition-colors"
            aria-label="Poprzedni slajd"
          >
            <img :src="arrowRight" alt="strzalkaWprawo">
          </button>
          
          <button
            type="button"
            class="flex items-center justify-center w-16 h-16 sm:w-20 sm:h-20 bg-white hover:bg-neutral-100 transition-colors"
            aria-label="Następny slajd"
          >
            <img :src="arrowLeft" alt="strzalkaWlewo">
          </button>
        </div>
      </div>
    </div>
  </section>
</template>