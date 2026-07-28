<script setup lang="ts">
import { nextTick, ref, onMounted, onBeforeUnmount } from 'vue'

import logo from '@/assets/icons/logo.svg'
import arrowDown from '@/assets/icons/menu/arrowDown.svg'
import search from '@/assets/icons/menu/search.svg'
import hamburger from '@/assets/icons/menu/hamburger.svg'
import close from '@/assets/icons/menu/close.svg'

interface NavItem {
  label: string
  icon?: string
  href: string
  children?: NavItem[]
}

interface FooterItem {
  label: string
  href: string
}

const navItems: NavItem[] = [
  {
    label: 'Oferta',
    icon: arrowDown,
    href: '#oferta',
    children: [
      { label: 'Projektowanie', href: '#oferta' },
      { label: 'Wykonanie', href: '#oferta' },
      { label: 'Doradztwo', href: '#oferta' },
      { label: 'Autorzy', href: '#o-firmie' },
    ],
  },
  { label: 'O firmie', href: '#o-firmie' },
  { label: 'Realizacje', href: '#realizacje' },
  { label: 'Kontakt', href: '#kontakt' },
]

const activeDropdown = ref<string | null>(null)
const searchOpen = ref(false)
const searchQuery = ref('')
const navRef = ref<HTMLElement | null>(null)
const searchInputRef = ref<HTMLInputElement | null>(null)
const searchInputMobileRef = ref<HTMLInputElement | null>(null)

const mobileMenuOpen = ref(false)

const isNavbarHidden = ref(false)
let lastScrollY = 0

const footerItems: FooterItem[] = [
  { label: 'Kontakt', href: '#kontakt' },
  { label: 'Instagram', href: 'https://instagram.com' },
  { label: 'Facebook', href: 'https://facebook.com' },
  { label: 'LinkedIn', href: 'https://linkedin.com' },
]

function toggleDropdown(label: string) {
  activeDropdown.value = activeDropdown.value === label ? null : label
}

function closeDropdown() {
  activeDropdown.value = null
}

function toggleSearch() {
  searchOpen.value = !searchOpen.value
  if (searchOpen.value) {
    activeDropdown.value = null
    nextTick(() => {
      searchInputRef.value?.focus()
      searchInputMobileRef.value?.focus()
    })
  } else {
    searchQuery.value = ''
  }
}

function closeSearch() {
  searchOpen.value = false
  searchQuery.value = ''
}

function toggleMobileMenu() {
  mobileMenuOpen.value = !mobileMenuOpen.value
  if (mobileMenuOpen.value) {
    closeSearch()
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
    closeDropdown()
  }
}

function handleDocumentClick(event: MouseEvent) {
  if (!navRef.value?.contains(event.target as Node)) {
    closeDropdown()
    closeSearch()
  }
}

function handleDocumentKeydown(event: KeyboardEvent) {
  if (event.key === 'Escape') {
    closeDropdown()
    closeSearch()
    if (mobileMenuOpen.value) {
      toggleMobileMenu()
    }
  }
}

function handleScroll() {
  const currentScrollY = window.scrollY

  if (currentScrollY <= 0) {
    isNavbarHidden.value = false
    lastScrollY = currentScrollY
    return
  }

  if (mobileMenuOpen.value) {
    isNavbarHidden.value = false
    lastScrollY = currentScrollY
    return
  }

  if (currentScrollY > lastScrollY && currentScrollY > 50) {
    isNavbarHidden.value = true
    closeDropdown()
    closeSearch()
  } else if (currentScrollY < lastScrollY) {
    isNavbarHidden.value = false
  }

  lastScrollY = currentScrollY
}

onMounted(() => {
  document.addEventListener('click', handleDocumentClick)
  document.addEventListener('keydown', handleDocumentKeydown)
  window.addEventListener('scroll', handleScroll, { passive: true })
})

onBeforeUnmount(() => {
  document.removeEventListener('click', handleDocumentClick)
  document.removeEventListener('keydown', handleDocumentKeydown)
  window.removeEventListener('scroll', handleScroll)
  document.body.style.overflow = ''
})
</script>

<template>
  <header
    :class="[
      'w-full bg-white fixed top-0 z-50 transition-transform duration-300 ease-in-out',
      isNavbarHidden ? '-translate-y-full' : 'translate-y-0',
    ]"
  >
    <nav ref="navRef" class="container-custom flex items-center justify-between relative py-6">
      <!-- Lewa strona -->
      <div class="shrink-0 flex items-center">
        <a href="/" class="inline-flex items-center">
          <img :src="logo" alt="Logo" class="h-4.75 w-[114.37px]" />
        </a>
      </div>

      <!-- Widok dekstopowy (od lg w górę) -->
      <div class="hidden lg:flex items-center">
        <!-- Kontener utrzymuje szerokość nawigacji -->
        <div class="relative flex items-center">
          <ul
            class="flex gap-x-12 transition-all duration-300 ease-out"
            :class="
              searchOpen
                ? 'opacity-0 pointer-events-none -translate-x-4'
                : 'opacity-100 pointer-events-auto translate-x-0'
            "
          >
            <li v-for="item in navItems" :key="item.label" class="relative text-[14px]">
              <template v-if="item.children">
                <button
                  type="button"
                  class="font-normal transition-colors duration-150 hover:text-default text-default"
                  :aria-expanded="activeDropdown === item.label"
                  @click="toggleDropdown(item.label)"
                >
                  <span class="inline-flex items-center gap-2">
                    <span>{{ item.label }}</span>
                    <img
                      v-if="item.icon"
                      :src="item.icon"
                      alt=""
                      aria-hidden="true"
                      class="transition-transform duration-200"
                      :class="activeDropdown === item.label ? 'rotate-180' : ''"
                    />
                  </span>
                </button>

                <!-- Dropdown Desktop -->
                <ul
                  :class="[
                    'absolute top-full left-0 mt-4 w-48 bg-white border border-base shadow-[0_4px_20px_-4px_rgba(0,0,0,0.05)] py-2 z-10 transition-all duration-200 ease-out',
                    activeDropdown === item.label
                      ? 'opacity-100 translate-y-0 pointer-events-auto'
                      : 'opacity-0 translate-y-2 pointer-events-none',
                  ]"
                >
                  <li v-for="child in item.children" :key="child.href">
                    <a
                      :href="child.href"
                      @click="closeDropdown(); if (mobileMenuOpen) toggleMobileMenu();"
                      class="block px-5 py-2.5 text-sm text-default/70 transition-colors duration-150 hover:text-default hover:bg-bg-base"
                    >
                      {{ child.label }}
                    </a>
                  </li>
                </ul>
              </template>

              <template v-else>
                <a
                  :href="item.href"
                  @click="closeDropdown(); if (mobileMenuOpen) toggleMobileMenu();"
                  class="font-normal transition-colors duration-150 hover:text-default text-default"
                >
                  {{ item.label }}
                </a>
              </template>
            </li>
          </ul>

          <!-- Wyszukiwarka Desktop -->
          <div
            class="absolute inset-0 flex items-center transition-all duration-300 ease-out"
            :class="
              searchOpen
                ? 'opacity-100 translate-x-0 pointer-events-auto'
                : 'opacity-0 translate-x-4 pointer-events-none'
            "
          >
            <input
              ref="searchInputRef"
              v-model="searchQuery"
              type="search"
              placeholder="Szukaj..."
              class="w-full py-1 text-sm text-default bg-transparent border-b border-bg-sand outline-none focus:ring-0 placeholder-default/50"
            />
          </div>
        </div>

        <!-- Lupa Desktop -->
        <div class="relative flex items-center ml-12">
          <button
            type="button"
            class="relative z-20 inline-flex items-center justify-center transition-transform duration-200 hover:scale-110 cursor-pointer"
            @click="toggleSearch"
            :aria-expanded="searchOpen"
            aria-label="Otwórz wyszukiwanie"
          >
            <img :src="search" alt="Szukaj" />
          </button>
        </div>
      </div>

      <!-- Widok mobile -->
      <div class="flex lg:hidden items-center gap-2 flex-1 justify-end relative">
        <!-- Pole wyszukiwania mobile -->
        <div
          class="flex-1 transition-all duration-300 ease-out ml-4 sm:ml-8 mr-1 overflow-hidden"
          :class="
            searchOpen
              ? 'opacity-100 pointer-events-auto max-w-full'
              : 'opacity-0 pointer-events-none max-w-0'
          "
        >
          <input
            ref="searchInputMobileRef"
            v-model="searchQuery"
            type="search"
            placeholder="Szukaj..."
            class="w-full py-1 text-sm text-default bg-transparent border-b border-bg-sand outline-none focus:ring-0 placeholder-default/50"
          />
        </div>

        <!-- Przycisk Lupy Mobile z animacja -->
        <button
          type="button"
          class="relative z-20 inline-flex items-center justify-center p-2 transition-all duration-300 ease-out hover:scale-110 active:scale-95 cursor-pointer"
          @click="toggleSearch"
          aria-label="Otwórz wyszukiwanie"
        >
          <img :src="search" alt="Szukaj" class="w-5 h-5 transition-transform duration-300" :class="searchOpen ? 'rotate-90' : 'rotate-0'" />
        </button>

        <!-- Przycisk Menu Mobilnego (Hamburger i X) -->
        <div
          class="transition-all duration-300 ease-out overflow-hidden flex items-center justify-center"
          :class="searchOpen ? 'max-w-0 opacity-0 pointer-events-none p-0' : 'max-w-10 opacity-100 p-1'"
        >
          <button
            type="button"
            class="relative z-20 inline-flex items-center justify-center text-default cursor-pointer"
            @click="toggleMobileMenu"
            aria-label="Otwórz menu"
          >
            <img v-if="!mobileMenuOpen" :src="hamburger" alt="Otwórz menu" class="h-6 w-6" />
            <img v-else :src="close" alt="Zamknij menu" class="h-6 w-6" />
          </button>
        </div>
      </div>

      <!-- Wysuwane menu mobile -->
      <div
        class="absolute top-full left-0 w-full bg-white lg:hidden transition-all duration-300 ease-in-out overflow-y-auto flex flex-col shadow-lg"
        style="height: calc(100dvh - 70px)"
        :class="
          mobileMenuOpen
            ? 'translate-x-0 opacity-100 border-t border-base'
            : 'translate-x-full opacity-0 pointer-events-none'
        "
      >
        <div class="flex flex-col min-h-full py-8 container-custom">
          <ul class="flex flex-col gap-8">
            <li v-for="item in navItems" :key="item.label" class="w-full">
              <template v-if="item.children">
                <button
                  type="button"
                  class="w-full flex items-center justify-between text-[22px] text-default transition-colors"
                  @click="toggleDropdown(item.label)"
                >
                  <span>{{ item.label }}</span>
                  <img
                    :src="arrowDown"
                    alt=""
                    aria-hidden="true"
                    class="h-5 w-5 opacity-60 transition-transform duration-200"
                    :class="activeDropdown === item.label ? 'rotate-180' : ''"
                  />
                </button>

                <!-- Podmenu dropdown -->
                <div
                  class="overflow-hidden transition-all duration-300 ease-in-out w-full"
                  :class="
                    activeDropdown === item.label
                      ? 'max-h-64 opacity-100 mt-4'
                      : 'max-h-0 opacity-0'
                  "
                >
                  <ul class="flex flex-col gap-4 pl-4 border-l border-base">
                    <li v-for="child in item.children" :key="child.href">
                      <a
                        :href="child.href"
                        @click="toggleMobileMenu()"
                        class="block text-[16px] text-default/70 hover:text-default"
                      >
                        {{ child.label }}
                      </a>
                    </li>
                  </ul>
                </div>
              </template>

              <template v-else>
                <a
                  :href="item.href"
                  @click="toggleMobileMenu()"
                  class="block w-full text-[22px] text-default transition-colors"
                >
                  {{ item.label }}
                </a>
              </template>
            </li>
          </ul>

          <!-- Skróty ze stopki -->
          <div class="mt-auto flex flex-wrap gap-x-6 pt-12 text-[14px] text-default/60">
            <a
              v-for="item in footerItems"
              :key="item.label"
              :href="item.href"
              @click="toggleMobileMenu()"
              class="hover:text-default transition-colors"
            >
              {{ item.label }}
            </a>
          </div>
        </div>
      </div>
    </nav>
  </header>
</template>
