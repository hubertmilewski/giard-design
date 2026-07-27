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
    href: '/oferta',
    children: [
      { label: 'Projektowanie', href: '/oferta/projektowanie' },
      { label: 'Wykonanie', href: '/oferta/wykonanie' },
      { label: 'Doradztwo', href: '/oferta/doradztwo' },
      { label: 'Autorzy', href: '/oferta/doradztwo' },
    ],
  },
  { label: 'O firmie', href: '/o-firmie' },
  { label: 'Realizacje', href: '/realizacje' },
  { label: 'Kontakt', href: '/kontakt' },
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
  { label: 'Kontakt', href: '/kontakt' },
  { label: 'Instagram', href: '#' },
  { label: 'Facebook', href: '#' },
  { label: 'LinkedIn', href: '#' },
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
      <div
        :class="[
          'transition-opacity duration-200',
          searchOpen ? 'opacity-0 pointer-events-none' : 'opacity-100 pointer-events-auto',
          'lg:opacity-100 lg:pointer-events-auto',
        ]"
      >
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
                  class="font-normal transition-colors duration-150 hover:text-slate-900"
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
                    'absolute top-full left-0 mt-4 w-48 bg-white border border-slate-100 shadow-[0_4px_20px_-4px_rgba(0,0,0,0.05)] py-2 z-10 transition-all duration-200 ease-out',
                    activeDropdown === item.label
                      ? 'opacity-100 translate-y-0 pointer-events-auto'
                      : 'opacity-0 translate-y-2 pointer-events-none',
                  ]"
                >
                  <li v-for="child in item.children" :key="child.href">
                    <a
                      :href="child.href"
                      class="block px-5 py-2.5 text-sm text-slate-500 transition-colors duration-150 hover:text-slate-900 hover:bg-slate-50/80"
                    >
                      {{ child.label }}
                    </a>
                  </li>
                </ul>
              </template>

              <template v-else>
                <a
                  :href="item.href"
                  class="font-normal transition-colors duration-150 hover:text-slate-900"
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
              class="w-full text-sm text-slate-900 bg-transparent border-none border-b border-slate-300 outline-none focus:ring-0 placeholder-slate-500"
            />
          </div>
        </div>

        <!-- Lupa Desktop -->
        <div class="relative flex items-center ml-12">
          <button
            type="button"
            class="relative z-20 inline-flex items-center justify-center transition-transform duration-200 hover:scale-110"
            @click="toggleSearch"
            :aria-expanded="searchOpen"
            aria-label="Otwórz wyszukiwanie"
          >
            <img :src="search" alt="Szukaj" />
          </button>
        </div>
      </div>

      <!-- Widok mobile -->
      <div class="flex lg:hidden items-center gap-4">
        <!-- Pole wyszukiwania mobile -->
        <div
          class="absolute inset-y-0 left-0 right-0 bg-white flex items-center z-10 transition-all duration-300 ease-out px-4"
          :class="
            searchOpen
              ? 'opacity-100 pointer-events-auto translate-x-0'
              : 'opacity-0 pointer-events-none -translate-x-4'
          "
        >
          <input
            ref="searchInputMobileRef"
            v-model="searchQuery"
            type="search"
            placeholder="Szukaj..."
            class="w-full py-2 text-sm text-slate-900 bg-transparent border-none border-b border-slate-300 outline-none focus:ring-0 placeholder-slate-500"
          />
        </div>

        <button
          type="button"
          class="relative z-20 inline-flex items-center justify-center p-2 transition-transform hover:scale-110"
          @click="toggleSearch"
          aria-label="Otwórz wyszukiwanie"
        >
          <img :src="search" alt="Szukaj" class="w-5 h-5" />
        </button>

        <!-- Przycisk Menu Mobilnego (Hamburger i X) -->
        <button
          type="button"
          class="relative z-20 inline-flex items-center justify-center text-slate-900"
          @click="toggleMobileMenu"
          aria-label="Otwórz menu"
        >
          <img v-if="!mobileMenuOpen" :src="hamburger" alt="Otwórz menu" class="h-6 w-6" />
          <img v-else :src="close" alt="Zamknij menu" class="h-6 w-6" />
        </button>
      </div>

      <!-- Wysuwane menu mobile -->
      <div
        class="absolute top-full left-0 w-full bg-white lg:hidden transition-all duration-300 ease-in-out overflow-y-auto flex flex-col shadow-lg"
        style="height: calc(100dvh - 70px)"
        :class="
          mobileMenuOpen
            ? 'translate-x-0 opacity-100 border-t border-slate-50'
            : 'translate-x-full opacity-0 pointer-events-none'
        "
      >
        <div class="flex flex-col min-h-full py-8 container-custom">
          <ul class="flex flex-col gap-8">
            <li v-for="item in navItems" :key="item.label" class="w-full">
              <template v-if="item.children">
                <button
                  type="button"
                  class="w-full flex items-center justify-between text-[22px] text-slate-900 transition-colors"
                  @click="toggleDropdown(item.label)"
                >
                  <span>{{ item.label }}</span>
                  <img
                    :src="arrowDown"
                    alt=""
                    aria-hidden="true"
                    class="h-5 w-5 text-slate-400 transition-transform duration-200"
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
                  <ul class="flex flex-col gap-4 pl-4 border-l border-slate-100">
                    <li v-for="child in item.children" :key="child.href">
                      <a
                        :href="child.href"
                        class="block text-[16px] text-slate-500 hover:text-slate-900"
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
                  class="block w-full text-[22px] text-slate-900 transition-colors"
                >
                  {{ item.label }}
                </a>
              </template>
            </li>
          </ul>

          <!-- Skróty ze stopki -->
          <div class="mt-auto flex flex-wrap gap-x-6 pt-12 text-[14px] text-slate-500">
            <a
              v-for="item in footerItems"
              :key="item.label"
              :href="item.href"
              class="hover:text-slate-900 transition-colors"
            >
              {{ item.label }}
            </a>
          </div>
        </div>
      </div>
    </nav>
  </header>
</template>
