<script setup>
const { data } = await useAsyncData('blog-tags', () => queryContent('blog').only(['tags']).find())

const tags = new Set(data.value.map(Object.values).flat(2))

const config = useRuntimeConfig()
useHead({
  title: `Tags | ${config.public.appName}`,
})
</script>

<template>
  <div class="container mx-auto">
    <h1 class="title pt-10 text-center">
      Tags
    </h1>

    <section class="blog-tags py-10">
      <ul class="flex flex-wrap gap-2">
        <template v-for="(t, i) in tags" :key="`tag-${i}`">
          <li class="tag px-4 py-2 rounded text-2xl whitespace-nowrap transition-all bg-gray-300 dark:bg-black hover:-translate-y-0.5">
            <span>#&nbsp;</span>
            <NuxtLink :to="`/blog/tags/${t}`" class="font-semibold">
              {{ t }}
            </NuxtLink>
          </li>
        </template>
      </ul>
    </section>
  </div>
</template>
