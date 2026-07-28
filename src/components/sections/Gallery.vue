<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue'
import Macy from 'macy'

import ContentBlock from '@/components/ui/ContentBlock.vue'
import Arrow from '@/assets/icons/button/buttonArrowDown.svg'

// Ładowanie obrazków (zamiast importu pojedynczych plików)
const imageModules = import.meta.glob('@/assets/images/ourProjects/*.webp', {
  eager: true,
}) as Record<string, { default: string }>

const masonryContainer = ref<HTMLElement | null>(null)
const isExpanded = ref(false)

const selectedImageIndex = ref<number | null>(null)
const activeProject = computed(() => {
  if (selectedImageIndex.value === null) return null
  return projects[selectedImageIndex.value] || null
})

// Sortowanie według numerów w nazwie plików image.webp = 1, image2.webp = 2 ...
const getFilenameNumber = (path: string) => {
  const match = path.match(/image(\d+)?\./i)
  if (!match) return 0
  return match[1] ? parseInt(match[1], 10) : 1
}

const projects = Object.entries(imageModules)
  .sort(([a], [b]) => getFilenameNumber(a) - getFilenameNumber(b))
  .map(([path, module]) => {
    const num = getFilenameNumber(path)
    return {
      id: num,
      src: module.default,
      alt: `Projekt ${num}`,
    }
  })

onMounted(() => {
  nextTick(() => {
    Macy({
      container: masonryContainer.value,
      trueOrder: true,
      waitForImages: true,
      margin: 40,
      columns: 3,
      breakAt: {
        1024: 2,
        640: 1,
      },
    })
  })
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})

function expandGallery() {
  isExpanded.value = true
}

function openLightbox(index: number) {
  selectedImageIndex.value = index
  document.body.style.overflow = 'hidden'
  document.documentElement.style.overflow = 'hidden'
}

function closeLightbox() {
  selectedImageIndex.value = null
  document.body.style.overflow = ''
  document.documentElement.style.overflow = ''
}

function prevImage() {
  if (selectedImageIndex.value !== null) {
    selectedImageIndex.value =
      (selectedImageIndex.value - 1 + projects.length) % projects.length
  }
}

function nextImage() {
  if (selectedImageIndex.value !== null) {
    selectedImageIndex.value = (selectedImageIndex.value + 1) % projects.length
  }
}

function handleKeydown(e: KeyboardEvent) {
  if (selectedImageIndex.value === null) return
  if (e.key === 'Escape') closeLightbox()
  if (e.key === 'ArrowLeft') prevImage()
  if (e.key === 'ArrowRight') nextImage()
}
</script>

<template>
  <section id="realizacje" class="w-full bg-bg-sand overflow-hidden">
    <div class="container-custom mx-auto fade-on-scroll">
      <div class="py-12 md:py-20 lg:pt-30 lg:pb-24">
        <ContentBlock descriptionClass="hidden">
          <template #tag>
            <span class="text-[12px] text-primary">Realizacje</span>
          </template>

          <template #title> Nasze <span class="italic font-sans">projekty</span> </template>
        </ContentBlock>
      </div>
    </div>

    <div class="relative w-full">
      <div
        class="w-full overflow-hidden transition-[max-height] duration-700 ease-in-out"
        :class="isExpanded ? 'max-h-[6000px]' : 'max-h-275 sm:max-h-380'"
      >
        <div ref="masonryContainer" class="w-full">
          <div
            v-for="(project, index) in projects"
            :key="project.id"
            class="cursor-pointer overflow-hidden group"
            @click="openLightbox(index)"
          >
            <img
              :src="project.src"
              :alt="project.alt"
              loading="lazy"
              decoding="async"
              width="450"
              height="450"
              class="w-full h-auto object-cover group-hover:scale-105 transition-transform duration-500"
            />
          </div>
        </div>
      </div>

      <div
        v-if="!isExpanded"
        class="absolute bottom-0 left-0 w-full h-72 sm:h-112.5 bg-linear-to-t from-bg-sand via-bg-sand/90 to-transparent flex items-end justify-center pb-8 sm:pb-15 pointer-events-none"
      >
        <div class="pointer-events-auto">
          <button
            type="button"
            class="inline-flex items-center justify-center gap-2 rounded-full border border-primary px-6 py-3 text-sm text-primary transition duration-200 hover:bg-primary/10 cursor-pointer"
            @click="expandGallery"
          >
            Rozwiń
            <img :src="Arrow" alt="strzalkaWdol" />
          </button>
        </div>
      </div>
    </div>

    <Teleport to="body">
      <Transition name="fade">
        <div
          v-if="selectedImageIndex !== null"
          class="fixed inset-0 z-50 flex items-center justify-center bg-black/60 backdrop-blur-sm p-4 sm:p-8 touch-none overscroll-none"
          @click.self="closeLightbox"
          @wheel.prevent
          @touchmove.prevent
        >
          <button
            type="button"
            class="absolute top-6 right-6 text-white text-3xl font-light hover:opacity-75 transition-opacity cursor-pointer p-2 z-10"
            aria-label="Zamknij"
            @click="closeLightbox"
          >
            ✕
          </button>

          <button
            type="button"
            class="absolute left-4 sm:left-8 text-white text-3xl sm:text-4xl hover:opacity-75 transition-opacity cursor-pointer p-3 z-10 bg-black/40 rounded-full w-12 h-12 flex items-center justify-center"
            aria-label="Poprzednie zdjęcie"
            @click="prevImage"
          >
            ‹
          </button>

          <div
            v-if="activeProject && selectedImageIndex !== null"
            class="relative max-w-[90vw] max-h-[85vh] flex flex-col items-center"
          >
            <img
              :src="activeProject.src"
              :alt="activeProject.alt"
              class="max-w-full max-h-[80vh] object-contain rounded-lg shadow-2xl transition-all duration-300"
            />
            <div class="mt-4 text-white/80 text-sm font-sans tracking-wide">
              {{ selectedImageIndex + 1 }} / {{ projects.length }}
            </div>
          </div>

          <button
            type="button"
            class="absolute right-4 sm:right-8 text-white text-3xl sm:text-4xl hover:opacity-75 transition-opacity cursor-pointer p-3 z-10 bg-black/40 rounded-full w-12 h-12 flex items-center justify-center"
            aria-label="Następne zdjęcie"
            @click="nextImage"
          >
            ›
          </button>
        </div>
      </Transition>
    </Teleport>
  </section>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
