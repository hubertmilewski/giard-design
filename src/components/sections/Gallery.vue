<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue'
import Macy from 'macy'

import ContentBlock from '@/components/ui/ContentBlock.vue'
import Arrow from '@/assets/icons/button/buttonArrowDown.svg'

const imageModules = import.meta.glob('@/assets/images/ourProjects/*.webp', {
  eager: true,
}) as Record<string, { default: string }>

const masonryContainer = ref<HTMLElement | null>(null)
const isExpanded = ref(false)

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
})

function expandGallery() {
  isExpanded.value = true
}
</script>

<template>
  <section class="w-full bg-bg-sand overflow-hidden">

    <div class="container-custom mx-auto">
      <div class="pt-30 pb-24">
        <ContentBlock titleClass="mt-[16px]" descriptionClass="hidden">
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
        :class="isExpanded ? 'max-h-[5000px]' : 'max-h-380'"
      >
        <div ref="masonryContainer" class="w-full">
          <div
            v-for="project in projects"
            :key="project.id"
            class="cursor-pointer overflow-hidden"
          >
            <img
              :src="project.src"
              :alt="project.alt"
              loading="lazy"
              decoding="async"
              width="450"
              height="450"
              class="w-full h-auto object-cover hover:scale-105 transition-transform duration-500"
            />
          </div>
        </div>
      </div>

      <div
        v-if="!isExpanded"
        class="absolute bottom-0 left-0 w-full h-112.5 bg-linear-to-t from-bg-sand via-bg-sand/90 to-transparent flex items-end justify-center pb-15 pointer-events-none"
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
  </section>
</template>
