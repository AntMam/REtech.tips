<template>
  <div class="relative w-full bg-gradient-to-b from-gray-100 to-white">
    <TheHeader />
    <div class="flex flex-col overlay bg-gray-900 relative w-full xs:h-84 h-full">
        <img
          :src="require(`/assets/images/${article.img}`)"
          :alt="article.alt"
          class="absolute h-full w-full object-cover opacity-20"
        />
        <div class="container w-full md:max-w-3xl mx-auto pt-20 text-white text-4xl font-bold uppercase mt-2">
          {{ article.title }}
        </div>
        <div class="container w-full md:max-w-3xl mx-auto pt-10 grid grid-flow-col grid-cols-12 auto-cols-max border py-9 rounded-b-2xl">
          <div
          v-for="(tag, id) in article.tags" :key="id"
          class="relative col-span-4 py-2 leading-none rounded-full text-xs font-medium uppercase text-white text-center md:text-xs lg:text-xs"
          :style="{ 'background-color': tags[tag].color }"
          >
            <NuxtLink :to="`/blog/tag/${tags[tag].slug}`">
            <span>{{tag}}</span>
            </NuxtLink>
          </div>
        </div>
    </div>
    <div class=" md:container px-4 flex flex-col w-full md:max-w-3xl mx-auto pt-20">
      <NuxtLink to="/">
        <p class="hover:underline mb-4">&#8592; Back to All Articles
        </p>
      </NuxtLink>
      <h1 class="font-bold text-4xl">{{ article.title }}</h1>
      <p class="mt-4 prose">{{ article.description }}</p>
      <p class="pb-4 mt-4">Post last updated: {{ formatDate(article.updatedAt) }}</p>
      <nuxt-content class="prose mt-4" :document="article" />

      <div class="comments mt-4 mb-4">
        <Disqus />
      </div>
      <!-- content author component -->
      <author :author="article.author" />
      <!-- Footer -->
      <Footer />
      <!-- prevNext component -->
      <h4 class="font-bold text-2xl text-center">More articles:</h4>
      <PrevNext :prev="prev" :next="next" class="mt-8 mb-8" />
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()
    const tagsList = await $content('tags')
      .only(['name', 'slug', 'color'])
      .where({ name: { $containsAny: article.tags } })
      .fetch()
    const tags = Object.assign({}, ...tagsList.map((s) => ({ [s.name]: s })))
    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
    return {
      article,
      tags,
      prev,
      next
    }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  },
  head() {
  return {
    title: this.article.title,
    meta: [
      // hid is used as unique identifier. Do not use `vmid` for it as it will not work
      {
        hid: 'og:article-description',
        name: 'og:description',
        content: this.article.description
      },
      {
          hid: 'og:title',
          property: 'og:title',
          content: this.article.title
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: this.article.img
        },
        {
          hid: 'og:image:secure_url',
          property: 'og:image:secure_url',
          content: this.article.img
        },
        {
          hid: 'og:image:alt',
          property: 'og:image:alt',
          content: this.article.title
        },
        {
          hid: 'og:type',
          property: 'og:type',
          content: 'article'
        },
        {
          hid: 'og:url',
          property: 'og:url',
          content: 'retech.tips/blog/' + this.article.slug
        },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: this.article.title 
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.article.description
        },
        {
          hid: 'twitter:image',
          name: 'twitter:image',
          content: this.article.img
        },
        {
          hid: 'twitter:image:alt',
          name: 'twitter:image:alt',
          content: this.article.title
        }
    ]
  }
}
}
  
</script>

<style>
</style>