<script setup>
import DarkToggle from '@/components/DarkToggle.vue'
import SideNavDrawer from '@/components/SideNavDrawer.vue'
import SearchModal from '@/components/SearchModal.vue'
import ShopCartDrawer from '@/components/ShopCartDrawer.vue'
import UserProfileDrawer from '@/components/UserProfileDrawer.vue'

const topLinks = [
  { label: 'English', dropdown: true },
  { link: 'https://discord.gg/x2tr4fJu', label: 'Community' },
  { link: '/support', label: 'Support' },
]
const socialIcons = [
  {
    icon: 'i-carbon:logo-discord',
    label: 'Discord',
    link: 'https://discord.gg/x2tr4fJu',
  },
  { icon: 'i-carbon:logo-instagram', label: 'Instagram', link: 'https://www.instagram.com/TokyoToolkit/' },
  { icon: 'i-carbon:logo-twitter', label: 'Twitter', link: 'https://twitter.com/TokyoToolkit' },
  { icon: 'i-carbon:logo-youtube', label: 'YouTube', link: 'https://www.youtube.com/channel/UCzV2BYxwNJlgc67Szapji0g' },
]
const navLinks = ref([
  { link: '/about', label: 'About' },
  {
    link: '/events',
    label: 'Events',
    dropdown: true,
    isOpen: false,
    menu: [
      { link: '/events/all', label: 'All' },
      { link: '/events/workshops', label: 'Workshops' },
    ],
  },
  { link: '/blog', label: 'Blog' },
  {
    link: '/shop',
    label: 'Shop',
    dropdown: true,
    isOpen: false,
    menu: [
      { link: { path: '/shop', query: { category: 'men\'s clothing' } }, label: 'Men\'s Clothing' },
      { link: { path: '/shop', query: { category: 'women\'s clothing' } }, label: 'Women\'s Clothing' },
    ],
  },
  { link: '/contact', label: 'Contact' },
])
const navIcons = [
  { component: SearchModal, label: 'Search' },
  { component: ShopCartDrawer, label: 'Shopping Cart' },
  { component: DarkToggle, label: 'Toggle Dark Mode' }, // component: ToggleColorMode,
  { link: '/auth/login', icon: 'i-carbon:locked', label: 'Login' },
  // { component: UserProfileDrawer, label: 'Profile' },
]
</script>

<template>
  <div class="site-top-header bg-gray-300 dark:bg-black">
    <div class="slice-anim header-wrapper container px-4 lg:px-0 h-10">
      <ul class="menu flex items-center">
        <button class="flex relative hover:text-teal-500" title="translate">
          <div class="i-carbon:translate" />
        </button>
        <li v-for="(n, i) in topLinks" :key="`top-nav-link-${i}`" class="hidden lg:inline-block">
          <button v-if="n.dropdown" :data-text="n.label" class="slice flex text-sm tracking-wider px-2.5 py-0.5 dropdown">
            {{ n.label }}<span class="dots" />
          </button>
          <div v-else class="slice-line">
            <NuxtLink :to="n.link" class="slice flex text-sm tracking-wider px-2.5 py-0.5" :data-text="n.label">
              {{ n.label }}
            </NuxtLink>
          </div>
        </li>
      </ul>
      <ul class="menu-social flex items-center">
        <li v-for="(n, i) in socialIcons" :key="`nav-btn-${i}`" :title="n.label">
          <a :href="n.link" target="_blank" class="flex relative hover:text-teal-500 px-2.5">
            <div :class="n.icon" />
            <span class="sr-only">{{ n.label }}</span>
          </a>
        </li>
      </ul>
    </div>
  </div>

  <header class="site-main-header bg-white dark:bg-black bg-opacity-40 dark:bg-opacity-40 shadow-lg sticky top-0 z-20">
    <div class="header-wrapper container px-4 lg:px-0 h-20">
      <div class="flex gap-2.5 items-center">
        <SideNavDrawer>
          <div class="hidden lg:inline-block">
            <div class="i-carbon:tools-alt" />
          </div>
          <div class="inline-block lg:hidden">
            <div class="i-carbon:menu " />
          </div>
        </SideNavDrawer>
        <NuxtLink to="/" class="site-logo rainbow-text font-black text-lg lg:text-2xl">
          Tokyo<span class="heading-font">Toolkit</span>
        </NuxtLink>
      </div>

      <nav class="flex items-center slice-anim">
        <ul class="menu flex items-center">
          <template v-for="(n, i) in navLinks" :key="`main-nav-link-${i}`">
            <li class="group relative hidden lg:inline-block">
              <div class="slice-line">
                <NuxtLink
                  :to="n.link"
                  :data-text="n.label"
                  class="slice flex font-semibold text-lg tracking-wider py-0.5 px-6"
                  :class="n.dropdown && 'dropdown'"
                >
                  {{ n.label }}
                  <span v-if="n.dropdown" class="dots" />
                </NuxtLink>
              </div>
              <div v-if="n.dropdown" class="dropdown-menu slice-anim group-hover:block h-auto hidden absolute">
                <ul class="menu top-0 w-40 bg-gray-300 dark:bg-black shadow-lg py-3">
                  <template v-for="(d, i) in n.menu" :key="`dropdown-${n.label}-li-${i}`">
                    <div class="slice-line">
                      <li class="text-sm hover:text-teal-600">
                        <NuxtLink :to="d.link" :data-text="d.label" class="slice px-4 py-2 block">
                          {{ d.label }}
                        </NuxtLink>
                      </li>
                    </div>
                  </template>
                </ul>
              </div>
            </li>
          </template>
        </ul>
        <ul class="menu-icon flex items-center">
          <template v-for="(n, i) in navIcons" :key="`nav-btn-${i}`">
            <li class="flex relative hover:text-teal-500 px-2.5" :title="n.label">
              <NuxtLink v-if="!!n.link" :to="n.link">
                <div :class="n.icon" />
              </NuxtLink>
              <component :is="n.component" v-else-if="n.component" />
              <button v-else>
                <div :class="n.icon" />
                <span class="sr-only">{{ n.label }}</span>
              </button>
            </li>
          </template>
        </ul>
      </nav>
    </div>
  </header>
</template>

<style scoped>
.site-main-header {
  backdrop-filter: saturate(180%) blur(20px);
}

.header-wrapper {
  margin-left: auto;
  margin-right: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  /* @apply container mx-auto flex items-center justify-between; */
}

.dropdown span.dots:after {
  content: '';
  position: absolute;
  display: block;
  bottom: 0;
  right: 50%;
  width: 3px;
  height: 3px;
  margin-left: -1px;
  border-radius: 1.5px;
  box-shadow: 5px 0 0 0, -5px 0 0 0, inset 0 0 0 3px;
  opacity: 0.4;
  transition: 0.2s opacity;
}

.dropdown-menu {
  left: 50%;
  transform: translateX(-50%);
}
.dropdown-menu ul.menu {
  display: flex;
  flex-direction: column;
  /* @apply flex flex-col; */
}

/* strike-through animation links */
.slice-anim ul.menu a.slice,
.slice-anim ul.menu button.slice {
  color: transparent;
}
.slice-anim a,
.slice-anim button {
  white-space: nowrap;
  font-family: 'Marcellus SC', Georgia, serif;
  transition: 0.5s;
  position: relative;
  font-weight: 500 !important;
}
.slice-anim a:before,
.slice-anim a:after,
.slice-anim button:before,
.slice-anim button:after {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  transition: 0.5s;
  transition-delay: 0.25s;
}
.slice-anim a:before,
.slice-anim button:before {
  -webkit-clip-path: polygon(0 0, 100% 0, 100% 50%, 0 50%);
  clip-path: polygon(0 0, 100% 0, 100% 50%, 0 50%);
}
.slice-anim a:after,
.slice-anim button:after {
  -webkit-clip-path: polygon(0 50%, 100% 50%, 100% 100%, 0 100%);
  clip-path: polygon(0 50%, 100% 50%, 100% 100%, 0 100%);
}
.slice-anim a:hover,
.slice-anim button:hover {
  text-decoration: none;
}
.slice-anim a:hover:before,
.slice-anim button:hover:before {
  transform: translateX(2px);
}
.slice-anim a:hover:after,
.slice-anim button:hover:after {
  opacity: 1 !important;
  transform: translateX(-2px);
}

.slice-anim ul.menu .slice-line {
  position: relative;
  overflow: hidden;
  /* @apply relative overflow-hidden; */
}
.slice-anim ul.menu .slice-line:before {
  content: '';
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 100%;
  height: 1px;
  left: -100%;
  transition: 0.5s;
  /* @apply bg-teal-500; */
  background-color: rgb(20, 184, 166);
}
.slice-anim ul.menu .slice-line:hover:before {
  left: 100%;
}

ul.menu > li a:before,
ul.menu > li a:after,
ul.menu > li button:before,
ul.menu > li button:after,
ul.menu > li span.dots:before,
ul.menu > li span.dots:after {
  padding: inherit;
  color: black;
}
.dark ul.menu > li a:before,
.dark ul.menu > li a:after,
.dark ul.menu > li button:before,
.dark ul.menu > li button:after,
.dark ul.menu > li span.dots:before,
.dark ul.menu > li span.dots:after {
  padding: inherit;
  color: white;
}
ul.menu li a:hover:before,
ul.menu li a:hover:after,
ul.menu li button:hover:before,
ul.menu li button:hover:after,
ul.menu li:hover span.dots:before,
ul.menu li:hover span.dots:after {
  /* @apply text-teal-500; */
  color: rgb(20, 184, 166);
}

ul.menu li a.router-link-exact-active:before,
ul.menu li a.router-link-exact-active:after,
ul.menu-icon li a.router-link-exact-active > svg {
  /* @apply text-teal-500; */
  color: rgb(20, 184, 166);
}
</style>
