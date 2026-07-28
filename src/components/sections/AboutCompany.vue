<script setup lang="ts">
import ContentBlock from '@/components/ui/ContentBlock.vue'
import Buttons from '@/components/ui/Buttons.vue'

import AboutImage from '@/assets/images/about-company.webp'
import arrow from '@/assets/icons/button/arrowOfert.svg'
</script>

<template>
  <section id="o-firmie" class="relative w-full bg-primary overflow-hidden">
    <div class="w-full h-72 sm:h-96 lg:hidden relative z-0">
      <img
        :src="AboutImage"
        alt="O firmie"
        class="w-full h-full object-cover"
        loading="lazy"
        decoding="async"
      />
    </div>

    <div class="hidden lg:block absolute inset-y-0 left-0 z-0 w-[calc(50vw-32px)]">
      <img
        :src="AboutImage"
        alt="O firmie"
        class="w-full h-full object-cover"
        loading="lazy"
        decoding="async"
      />
    </div>

    <div class="container-custom mx-auto relative z-10">
      <div class="grid grid-cols-1 lg:grid-cols-12 items-center">
        <div class="hidden lg:block lg:col-span-7"></div>

        <div class="w-full lg:col-span-5 py-12 sm:py-16 lg:py-32 px-4 sm:px-6 md:px-8 lg:px-0">
          <div class="max-w-130 mx-auto lg:mx-0 fade-on-scroll">
            <ContentBlock titleClass="text-base" descriptionClass="text-base mt-10 lg:mt-8">
              <template #tag>
                <span class="text-[12px] text-base"> O firmie </span>
              </template>

              <template #title>
                Tworzymy <br />
                <span class="italic">z pasją</span>
              </template>

              <template #description>
                Każdy projekt to nowe wyzwanie. Dlatego nasz zespół tworzą wykwalifikowani
                projektanci oraz architekci, których zadaniem jest rozpoznanie i realizacja potrzeb
                każdego Klienta. Nasza specjalizacja to przestrzenie nowoczesne, które
                charakteryzuje minimalizm, geometria i elegancka prostota. Tworzymy ogrody
                małoobsługowe, dostosowane do współczesnego trybu życia.
              </template>

              <template #actions>
                <Buttons primaryHref="#kontakt">
                  <template #secondary-base>
                    Poznaj nas bliżej
                    <img :src="arrow" alt="" class="ml-2 filter brightness-0 invert" />
                  </template>
                </Buttons>
              </template>
            </ContentBlock>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
