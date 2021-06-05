<template>
  <div class="w-full px-2 xs:mb-6 md:mb-12 article-card border-3 border-gray-500 justify-center">
    <NuxtLink
      :to="`/blog/author/${author.name}`"
      class="flex p-2 transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md"
    >
      <img
        v-if="author.img"
        class="h-40 rounded-full"
         :src="require(`/assets/images/${author.img}`)"
      />
      <div class="flex flex-col m-4 justify-center">
        <h4 class="font-semibold">Author</h4>

        <p class=" mt-4">{{ author.name }}</p>
        <p>{{ author.bio }}</p>
      </div>
    </NuxtLink>
  </div>
</template>

<script>
export default {
  props: {
    author: {
      type: Object,
      required: true
    }
  }
}
</script>