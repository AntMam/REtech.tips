<template>
    <div class="relative w-full bg-gradient-to-b from-gray-100 to-white">
      <TheHeader />
      <div class="overlay" :style="{ 'background-color': tag.color }">
        <div class="relative w-full xs:h-84 h-full">
        <img
          :src="require(`/assets/images/${tag.img}`)"
          :alt="tag.name"
          class="absolute h-full w-full object-cover opacity-40"
        />
        </div>
        <div class="absolute top-32 left-64 right-32 text-white">
        <NuxtLink to="/"></NuxtLink>
        <div class="mt-20 flex flex-col text-sm">
          <div class="relative w-full xs:h-84 lg:h-full">
            <h1 class="text-4xl font-bold uppercase mb-2">
              {{ tag.name }}
            </h1>
            <p class="mb-4 text-lg font-bold">{{ tag.description }}</p>
            <nuxt-content :document="tag" />
          </div>
        </div>
      </div>
      </div>
      <div class="relative xs:py-8 xs:px-8 lg:py-16 lg:px-16 w-full h-full markdown-body">
      
      <NuxtLink to="/">
      <p class="hover:underline mb-4">&#60; Back to All Articles</p>
      </NuxtLink>
      <h3 class="mb-4 font-bold text-4xl">Articles tagged {{ tag.name }}:</h3>
  
      <div class="grid grid-cols-12 col-span-12 gap-7">
        <div 
        v-for="article of articles" :key="article.slug"
        class="mt-8 flex flex-col items-start h-45 col-span-12 overflow-hidden shadow-sm rounded-xl md:col-span-6 lg:col-span-6 transition duration-200 ease-out transform hover:scale-105"
        >
          <NuxtLink 
            :to="{ name: 'blog-slug', params: { slug: article.slug } }"
          >
            <img class="object-cover w-full shadow-sm max-h-56" :src="require(`/assets/images/${article.img}`)">
            <div class="relative flex flex-col items-start col-span-12 px-6 bg-white border border-t-0 border-gray-200 py-6 rounded-b-2xl">
                <h2 class="text-base font-bold sm:text-lg md:text-xl">{{article.title}}</h2>
                <p class="markdown mt-2">{{article.description}}</p>
                <p class="font-bold text-gray-600 text-sm mt-5">{{ formatDate(article.updatedAt) }}</p>
            </div>
          </NuxtLink>
        </div>
      </div> 
      </div>
      <section id="footer">
        <Footer />
      </section>
    </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const tags = await $content('tags')
      .where({ slug: { $contains: params.tag } })
      .limit(1)
      .fetch()
   
    const tag = tags.length > 0 ? tags[0] : {}
    
    const articles = await $content('articles', params.slug)
      .where({ tags: { $contains: tag.name } })
      .sortBy('createdAt', 'asc')
      .fetch()
    return {
      articles,
      tag
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
    title: this.tag.name + ' - real estate tech tips',
    meta: [
      // hid is used as unique identifier. Do not use `vmid` for it as it will not work
      {
        hid: 'description',
        name: 'description',
        content: this.tag.description
      }
    ]
  }
}
}
</script>