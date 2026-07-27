<script setup lang="ts">
import arrow from '@/assets/icons/arrowOfert.svg'

interface Props {
  iconSrc?: string
  title: string
  description: string
  linkHref?: string
  linkText?: string
  cardClass?: string
}

withDefaults(defineProps<Props>(), {
  linkHref: '#',
  linkText: 'Dowiedz się więcej',
  cardClass: '',
})
</script>

<template>
  <article
    :class="[
      'bg-white rounded-[28px] px-6 py-8 sm:px-8 sm:py-10 lg:px-10 lg:py-12.5 flex flex-col items-start h-full',
      cardClass,
    ]"
  >
    <div class="mb-8 flex items-center justify-center w-10 h-10">
      <slot name="icon">
        <img
          v-if="iconSrc"
          :src="iconSrc"
          alt="Ikona usługi"
          class="w-full h-full object-contain"
        />
      </slot>
    </div>

    <!-- Tytuł -->
    <h3 class="font-heading font-medium mb-3 text-[18px] sm:text-[20px] md:text-[22px] lg:text-3xl">
      {{ title }}
    </h3>

    <p class="text-[14px] leading-relaxed mb-6 sm:mb-10 md:mb-12 lg:mb-15.75">
      {{ description }}
    </p>

    <a
      :href="linkHref"
      class="group inline-flex items-center gap-2.5 text-primary border-b border-primary pb-1 transition-all hover:text-primary/90 hover:border-primary/90"
    >
      {{ linkText }}

      <img
        :src="arrow"
        alt="Strzałka w prawo"
        class="transition-transform group-hover:translate-x-1"
      />
    </a>
  </article>
</template>
