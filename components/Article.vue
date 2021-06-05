<template>
<div>
    <NuxtLink 
        :to="{ name: 'blog-slug', params: { slug: article.slug } }"
    >
      <img class="object-cover w-full shadow-sm max-h-56" :src="require(`/assets/images/${article.img}`)">
      <div class="relative flex flex-col items-start grid grid-cols-12 px-6 bg-white border border-t-0 border-gray-200 py-7 rounded-b-2xl">
          <div
          v-for="(tag, id) in article.tags" :key="id"
          class="bg-yellow-400 relative top-0 col-span-4 -mt-10 px-1 py-1.5 leading-none rounded-full text-m font-medium uppercase text-white text-center md:text-xs lg:text-xs"
          :style="{ 'background-color': tags[tag].color }"
          >
            <span>{{tag}}</span>
          </div>
      </div>
        <div class="relative flex flex-col -mt-4 items-start col-span-12 px-6 bg-white border border-t-0 border-gray-200 py-2 rounded-b-2xl">
            <h2 class="text-base font-bold sm:text-2xl md:text-2xl mb-2">{{article.title}}</h2>
            <p class="markdown">{{article.description}}</p>
            <p class="font-bold text-gray-600 text-xs mt-5">{{ formatDate(article.updatedAt) }}</p>
        </div>
    </NuxtLink>
</div>
</template>

<script>
export default {
  props: {
    article: {
      type: Object,
      required: true
    },
    tags: {
        type: Object,
        required: false
    }
  },
  methods: {
    formatDate(date) {
        const options = { year: 'numeric', month: 'long', day: 'numeric' }
        return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>