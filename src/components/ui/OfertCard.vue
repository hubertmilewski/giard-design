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
      'bg-white rounded-[28px] px-10 py-12.5 flex flex-col items-start h-full',
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
    <h3 class="text-3xl font-heading font-medium mb-3">
      {{ title }}
    </h3>

    <p class="text-[14px] leading-relaxed mb-15.75">
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
