<template>
  <div class="w-full p-6 transition-shadow duration-150 ease-in-out shadow-md hover:shadow-lg mb-6">
    <NuxtLink
      :to="`/blog/author/${author.name}`"
      class="p-2"
    >
      <img
        v-if="author.img"
        class="h-40 mx-auto rounded-full"
         :src="require(`/assets/images/${author.img}`)"
      />
      <div class=" flex-col m-4">
        <h4 class="font-bold">Author</h4>

        <p class="mt-4 mb-2 font-semibold">{{ author.name }}</p>
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