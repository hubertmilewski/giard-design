<script setup lang="ts">
import ContentBlock from '@/components/ui/ContentBlock.vue'
import OfertCard from '@/components/ui/OfertCard.vue'

import Pen from '@/assets/icons/pencile.svg'
import Eye from '@/assets/icons/eye.svg'
import Stars from '@/assets/icons/stars.svg'
</script>

<template>
  <main class="w-full bg-bg-base">
    <section class="container-custom mx-auto pt-12 md:pt-20 lg:pt-30 pb-12 md:pb-20 lg:pb-40">
      <div class="w-full mb-12 md:mb-24 px-4 sm:px-6 lg:px-0">
        <ContentBlock
          titleClass="text-[32px] md:text-[40px] lg:text-[48px]"
          descriptionClass="mt-4 md:mt-8 max-w-[730px]"
        >
          <template #tag> Oferta </template>
          <template #title> Działamy <span class="italic">kompleksowo</span></template>
          <template #description>
            Oferujemy kompletną obsługę inwestycji terenów zielonych. Projektujemy nowoczesne ogrody
            przydomowe oraz rezydencjonalne. Stworzymy dla Ciebie projekt, zwizualizujemy go i
            wcielimy w życie, a na każdym etapie posłużymy radą i wieloletnim doświadczeniem.
          </template>
        </ContentBlock>
      </div>

      <div
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 md:gap-10 lg:gap-16 px-4 sm:px-6 lg:px-0"
      >
        <OfertCard
          :iconSrc="Pen"
          title="Projekty"
          description="Zaprojektujemy Twój ogród w nowoczesnym stylu i z najlepszym wykorzystaniem istniejącej przestrzeni."
          linkHref="/projekty"
        />

        <OfertCard
          :iconSrc="Eye"
          title="Wizualizacje"
          description="Przedstawimy Ci projekty koncepcyjne w postaci wirtualnego spaceru animowanego w technologii 3D."
          linkHref="/projekty"
        />

        <OfertCard
          :iconSrc="Stars"
          title="Realizacje"
          description="Zrealizujemy Twoje marzenie przy użyciu najnowszych rozwiązań i zaawansowanych technologii."
          linkHref="/projekty"
        />
      </div>
    </section>
  </main>
</template>
