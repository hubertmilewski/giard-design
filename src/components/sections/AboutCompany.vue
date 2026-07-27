<script setup lang="ts">
import ContentBlock from '@/components/ui/ContentBlock.vue'
import Buttons from '@/components/ui/Buttons.vue'

import AboutImage from '@/assets/images/about-company.webp'
import arrow from '@/assets/icons/arrowOfert.svg'
</script>

<template>
  <section class="relative w-full bg-primary overflow-hidden">

    <div class="w-full h-72 sm:h-96 lg:hidden relative z-0">
      <img :src="AboutImage" alt="O firmie" class="w-full h-full object-cover" />
    </div>

    <div class="hidden lg:block absolute inset-y-0 left-0 z-0 w-[calc(50vw-32px)]">
      <img :src="AboutImage" alt="O firmie" class="w-full h-full object-cover" />
    </div>

    <div class="container-custom mx-auto md:ml-24 relative z-10 flex">
      
      <div class="hidden lg:block w-[calc(50%+32px)] shrink-0"></div>
      
      <div class="w-full lg:w-125 py-16 lg:py-33.75 px-4 sm:px-6 lg:px-0 flex items-center">
        <ContentBlock titleClass="text-white" descriptionClass="text-white mt-[40px] max-w-[596px]">
          <template #tag>
            <span class="text-[12px] text-white"> O firmie </span>
          </template>

          <template #title>
            Tworzymy <br />
            <span class="italic">z pasją</span>
          </template>

          <template #description>
            Każdy projekt to nowe wyzwanie. Dlatego nasz zespół tworzą wykwalifikowani projektanci
            oraz architekci, których zadaniem jest rozpoznanie i realizacja potrzeb każdego Klienta.
            Nasza specjalizacja to przestrzenie nowoczesne, które charakteryzuje minimalizm,
            geometria i elegancka prostota. Tworzymy ogrody małoobsługowe, dostosowane do
            współczesnego trybu życia.
          </template>

          <template #actions>
            <Buttons primaryHref="/kontakt">
              <template #secondary-white>
                Poznaj nas bliżej
                <img :src="arrow" alt="" class="ml-2 filter brightness-0 invert" />
              </template>
            </Buttons>
          </template>
        </ContentBlock>
      </div>
      
    </div>
  </section>
</template>