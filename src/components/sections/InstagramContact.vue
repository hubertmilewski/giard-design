<script setup lang="ts">
import Buttons from '@/components/ui/Buttons.vue'
</script>

<template>
  <section id="kontakt" class="w-full bg-bg-base overflow-hidden">
    <div class="mx-auto w-full max-w-360 px-4 py-8 sm:px-6 sm:py-12 lg:px-50 lg:py-15">
      
      <div 
        class="w-full bg-primary flex flex-col lg:flex-row items-start sm:items-center justify-center px-6 py-10 sm:px-12 sm:py-14 lg:h-88 lg:gap-x-16 lg:px-0 lg:py-0 fade-on-scroll"
      >
        <!-- Lewa strona -->
        <div class="w-full sm:max-w-md lg:max-w-none lg:w-auto text-left">
          <h2 class="font-heading text-[26px] sm:text-[32px] lg:text-[40px] leading-tight text-base">
            Zostańmy w kontakcie!<br />
            Znajdziesz nas na <span class="italic font-medium">Instagramie.</span>
          </h2>
        </div>

        <!-- Prawa strona -->
        <div class="w-full sm:max-w-md lg:max-w-none lg:w-auto mt-8 sm:mt-10 lg:mt-0 flex flex-col items-start gap-5 sm:gap-6">
          <p class="text-[15px] sm:text-[16px] text-base leading-relaxed">
            Śledź nasze<br class="hidden sm:block" />
            najnowsze realizacje!
          </p>

          <Buttons secondaryHref="https://instagram.com">
            <template #primary-base>Instagram</template>
          </Buttons>
        </div>
      </div>
      
    </div>
  </section>
</template>