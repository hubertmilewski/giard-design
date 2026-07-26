<script setup lang="ts">
import ContentBlock from '@/components/ui/ContentBlock.vue' //
</script>

<template>
  <main class="w-full bg-bg-base">
    <section class="container-custom mx-auto pt-16 lg:pt-38.5 pb-16 lg:pb-32">

      <div class="lg:pl-30 w-full ">        
        <ContentBlock titleClass="text-[40px] lg:text-[60px] leading-[1.1] text-slate-900" descriptionClass="mt-8">
          <template #tag> Oferta </template>
          <template #title> Działamy <i class="italic">kompleksowo</i></template>
          <template #description>
            Oferujemy kompletną obsługę inwestycji terenów zielonych. Projektujemy nowoczesne ogrody
            przydomowe oraz rezydencjonalne. Stworzymy dla Ciebie projekt, zwizualizujemy go i wcielimy
            w życie, a na każdym etapie posłużymy radą i wieloletnim doświadczeniem.
          </template>
        </ContentBlock>
      </div>

    </section>
  </main>
</template>