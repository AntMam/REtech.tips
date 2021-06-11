<template>
<div>
<TheHeader />
<section id="topics" class="relative w-full bg-white">
    <div class="absolute w-full h-32 bg-gradient-to-b from-gray-100 to-white"></div>
    <div class="relative w-full px-3 py-10 mx-auto sm:py-6 md:py-12 md:px-8 max-w-7xl">

        <h1 class="mb-2 text-4xl font-extrabold leading-none text-gray-900 lg:text-3xl xl:text-4xl sm:mb-3"><a href="#_">Real Estate Tech Tips</a></h1>
        <p class="text-lg font-medium text-gray-800">Discover ways technology can empower your real estate processes. See our latest articles on strategies, tutorials, and more.</p>
        <hr class="mt-7">
        <h3 class="mb-1 mt-10 text-2xl font-bold leading-none text-gray-900 lg:text-2xl xl:text-2xl">Browse by Topic:</h3>
        <div class="flex grid h-1/2 grid-cols-12 gap-4 pb-3 mt-8 sm:mt-6">
            <div 
            v-for="tag of tags"
            :key="tag.slug"
            class="block transform h-64 transition duration-200 hover:scale-105 flex flex-col items-start col-span-12 rounded-xl group md:col-span-6 xl:col-span-3 "
            :style="{ 'background-color': tag.color }"
            >
                <NuxtLink :to="`/blog/tag/${tag.slug}`" class="absolute w-full h-full py-8 text-white px-5">
                    <h2 class="mb-3 text-2xl font-bold">{{ tag.name }} </h2>
                    <p class="mb-2 text-md text-white-100 opacity-100 md:text-md">{{ tag.description }}</p>
                </NuxtLink>
            </div>
        </div>
    </div>
</section>

  
<section class="relative w-full bg-white">

    <div class="absolute w-full h-32 bg-gradient-to-b from-gray-100 to-white"></div>
    <div class="relative w-full px-5 py-10 mx-auto sm:py-8 md:py-12 md:px-8 max-w-7xl">
      <h3 class="mb-1 mt-5 text-2xl font-bold leading-none text-gray-900 lg:text-2xl xl:text-2xl">All Blogs:</h3>
      <div class="grid grid-cols-12 col-span-12 gap-7">
        <div 
        v-for="article of articles" :key="article.slug"
        class="mt-8 flex flex-col items-start h-45 col-span-12 overflow-hidden shadow-sm rounded-xl md:col-span-6 lg:col-span-6 transition duration-200 ease-out transform hover:scale-105">
          <Article :article="article" :tags="tags"></Article>
        </div>
      </div> 
    </div>
</section>

<section id="footer">
  <Footer />
</section>

</div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
   
    const articles = await $content('articles', params.slug)
      .only(['title', 'description', 'img', 'slug', 'author', 'tags', 'updatedAt'])
      .sortBy('updatedAt', 'desc')
      .fetch()

    const tagsList = await $content('tags')
      .only(['name', 'slug', 'description', 'img', 'color'])
      .fetch()

    const tags = Object.assign({}, ...tagsList.map((s) => ({ [s.name]: s })))
   
    return {
      articles,
      tags
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
    meta: [
      // hid is used as unique identifier. Do not use `vmid` for it as it will not work
         {
          hid: 'og:title',
          property: 'og:title',
          content: 'REtech.tips'
        },
         {
          hid: 'og:url',
          property: 'og:url',
          content: 'retech.tips'
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: this.articles[0].img
        },
        {
          hid: 'og:image:secure_url',
          property: 'og:image:secure_url',
          content: this.articles[0].img
        },
        {
          hid: 'twitter:image',
          name: 'twitter:image',
          content: this.articles[0].img
        },
    ]
  }
}
}
</script>

