<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import ContentBlock from '@/components/ui/ContentBlock.vue'
import Buttons from '@/components/ui/Buttons.vue'

import buttonArrowDown from '@/assets/icons/button/buttonArrowDown.svg'
import arrowLeft from '@/assets/icons/slider/arrowLeft.svg'
import arrowRight from '@/assets/icons/slider/arrowRight.svg'

import IntroImage from '@/assets/images/intro-image.webp'
import IntroImage2 from '@/assets/images/intro-image2.webp'
import IntroImage3 from '@/assets/images/intro-image3.webp'

type Slide = { id: number; src: string; alt: string }

const slides: Slide[] = [
  { id: 1, src: IntroImage, alt: 'Zdjecie ogrodu - realizacja 1' },
  { id: 2, src: IntroImage2, alt: 'Zdjecie ogrodu - realizacja 2' },
  { id: 3, src: IntroImage3, alt: 'Zdjecie ogrodu - realizacja 3' },
]

const currentIndex = ref(0)
const currentSlide = computed<Slide>(() => slides[currentIndex.value] ?? slides[0]!)

const transitionName = ref('slide-next')
let autoSlideInterval: number | undefined

function startAutoSlide() {
  stopAutoSlide()
  autoSlideInterval = window.setInterval(() => {
    nextSlide(true)
  }, 5000)
}

function stopAutoSlide() {
  if (autoSlideInterval) {
    clearInterval(autoSlideInterval)
    autoSlideInterval = undefined
  }
}

onMounted(() => {
  startAutoSlide()
})

onUnmounted(() => {
  stopAutoSlide()
})

const nextSlide = (isAuto = false) => {
  transitionName.value = 'slide-next'
  currentIndex.value = (currentIndex.value + 1) % slides.length
  if (!isAuto) {
    startAutoSlide()
  }
}

const prevSlide = () => {
  transitionName.value = 'slide-prev'
  currentIndex.value = (currentIndex.value - 1 + slides.length) % slides.length
  startAutoSlide()
}
</script>

<template>
  <section class="w-full bg-bg-sand overflow-hidden">
    <div class="grid grid-cols-1 lg:grid-cols-12 min-h-screen lg:min-h-184.25 items-center">
      <!-- Lewa strona -->
      <div
        class="lg:col-span-6 flex flex-col justify-center py-16 lg:py-0 px-4 sm:px-4 lg:px-0 lg:pl-[calc((100vw-1440px)/2+89px)] min-h-[60vh] lg:min-h-0"
      >
        <ContentBlock
          :isH1="true"
          titleClass="text-[40px] lg:text-[60px] leading-[1.1]"
          descriptionClass="mt-11 max-w-[489px]"
        >
          <template #title>
            Nowoczesna aranżacja <br class="hidden lg:block" />Twojego ogrodu
          </template>

          <template #description>
            Marka GiardDesign to wieloletnie doświadczenie i wysoka estetyka realizacji. Oferujemy
            kompleksowy zakres usług z indywidualnym podejściem do każdego projektu.
          </template>

          <template #actions>
            <Buttons primaryHref="#kontakt" secondaryHref="#realizacje">
              <template #primary>Skontaktuj się z nami</template>
              <template #secondary>
                Zobacz nasze realizacje
                <img class="ml-2" :src="buttonArrowDown" alt="arrowDown" />
              </template>
            </Buttons>
          </template>
        </ContentBlock>
      </div>

      <!-- Prawa strona -->
      <div
        class="lg:col-span-6 self-stretch relative min-h-[40vh] lg:min-h-0 w-full overflow-hidden bg-bg-sand"
      >
        <Transition :name="transitionName">
          <img
            :key="currentSlide.id"
            :src="currentSlide.src"
            :alt="currentSlide.alt"
            class="absolute inset-0 w-full h-full object-cover"
            fetchpriority="high"
            loading="eager"
            decoding="async"
            width="1376"
            height="1474"
          />
        </Transition>

        <!-- Slider -->
        <div class="absolute bottom-0 right-0 flex bg-base z-10">
          <button
            type="button"
            class="flex items-center justify-center w-16 h-16 sm:w-20 sm:h-20 bg-base hover:bg-neutral-100 transition-colors cursor-pointer"
            aria-label="Poprzedni slajd"
            @click="prevSlide"
          >
            <img :src="arrowRight" alt="strzalkaWprawo" />
          </button>

          <button
            type="button"
            class="flex items-center justify-center w-16 h-16 sm:w-20 sm:h-20 bg-base hover:bg-neutral-100 transition-colors cursor-pointer"
            aria-label="Następny slajd"
            @click="nextSlide(false)"
          >
            <img :src="arrowLeft" alt="strzalkaWlewo" />
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.slide-next-enter-active,
.slide-next-leave-active,
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.slide-next-enter-from {
  transform: translateX(100%);
}
.slide-next-leave-to {
  transform: translateX(-100%);
}

.slide-prev-enter-from {
  transform: translateX(-100%);
}
.slide-prev-leave-to {
  transform: translateX(100%);
}

.slide-next-enter-to,
.slide-next-leave-from,
.slide-prev-enter-to,
.slide-prev-leave-from {
  transform: translateX(0);
}
</style>
