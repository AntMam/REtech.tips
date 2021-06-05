<template>
<div>
<div class="absolute w-full bg-gradient-to-b from-gray-100 to-white">
  <TheHeader />
    <div class="relative w-1/4 px-3 py-5 mx-auto sm:py-4 md:py-6 md:px-8 max-w-2xl max-h-1xl">
      <img
        :src="require(`/assets/images/${articles[0].author.img}`)"
        :alt="articles[0].author.name"
        class="relative rounded-full h-1/10"
      />
    </div>
    <div class="relative flex mx-auto px-3 py-3 w-full text-2xl justify-center mb-2">
      <h2> {{articles[0].author.name}} </h2>
    </div>
    <div class="relative flex mx-auto px-3 py-5 w-3/4 justify-center text-lg mb-4">
      <p> {{articles[0].author.bio}} </p>
    </div>
    <div class="flex mx-auto justify-center">
      <p> Feel Free to Reach Out: </p>
    </div>
    <div class="flex mx-auto justify-center py-6">
      <div class="grid grid-cols-6 items-center px-6"
        <a :href="`https://github.com/${articles[0].author.github}`" target="_blank">
          <svg viewBox="0 0 16 16" xmlns="http://www.w3.org/2000/svg" class="fill-current h-12 w-12">
            <path fill="black" d="M7.999,0.431c-4.285,0-7.76,3.474-7.76,7.761 c0,3.428,2.223,6.337,5.307,7.363c0.388,0.071,0.53-0.168,0.53-0.374c0-0.184-0.007-0.672-0.01-1.32 c-2.159,0.469-2.614-1.04-2.614-1.04c-0.353-0.896-0.862-1.135-0.862-1.135c-0.705-0.481,0.053-0.472,0.053-0.472 c0.779,0.055,1.189,0.8,1.189,0.8c0.692,1.186,1.816,0.843,2.258,0.645c0.071-0.502,0.271-0.843,0.493-1.037 C4.86,11.425,3.049,10.76,3.049,7.786c0-0.847,0.302-1.54,0.799-2.082C3.768,5.507,3.501,4.718,3.924,3.65 c0,0,0.652-0.209,2.134,0.796C6.677,4.273,7.34,4.187,8,4.184c0.659,0.003,1.323,0.089,1.943,0.261 c1.482-1.004,2.132-0.796,2.132-0.796c0.423,1.068,0.157,1.857,0.077,2.054c0.497,0.542,0.798,1.235,0.798,2.082 c0,2.981-1.814,3.637-3.543,3.829c0.279,0.24,0.527,0.713,0.527,1.437c0,1.037-0.01,1.874-0.01,2.129 c0,0.208,0.14,0.449,0.534,0.373c3.081-1.028,5.302-3.935,5.302-7.362C15.76,3.906,12.285,0.431,7.999,0.431z">
            </path>
          </svg>
        </a>
        <a :href="`mailto:${articles[0].author.email}`">
          <svg class="fill-current h-12 w-12 pl-2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" viewBox="0 0 350 350" xml:space="preserve">
          <g id="icon" style="stroke: none; stroke-width: 1; stroke-dasharray: none; stroke-linecap: butt; stroke-linejoin: miter; stroke-miterlimit: 10; fill: none; fill-rule: nonzero; opacity: 1;" transform="translate(-1.9444444444444287 -1.9444444444444287) scale(3.89 3.89)" >
            <path d="M 45 42.768 l 45 -20.785 v -3.587 c 0 -3.186 -2.582 -5.768 -5.768 -5.768 H 5.768 C 2.582 12.627 0 15.209 0 18.395 v 3.587 L 45 42.768 z" style="stroke: none; stroke-width: 1; stroke-dasharray: none; stroke-linecap: butt; stroke-linejoin: miter; stroke-miterlimit: 10; fill: rgb(0,0,0); fill-rule: nonzero; opacity: 1;" transform=" matrix(1 0 0 1 0 0) " stroke-linecap="round" />
            <path d="M 45 52.626 L 0 31.84 v 39.765 c 0 3.186 2.582 5.768 5.768 5.768 h 78.464 c 3.186 0 5.768 -2.582 5.768 -5.768 V 31.84 L 45 52.626 z" style="stroke: none; stroke-width: 1; stroke-dasharray: none; stroke-linecap: butt; stroke-linejoin: miter; stroke-miterlimit: 10; fill: rgb(0,0,0); fill-rule: nonzero; opacity: 1;" transform=" matrix(1 0 0 1 0 0) " stroke-linecap="round" />
          </g>
          </svg>
        </a>
      </div>
    <Footer />
    </div>
    <div>
  </div>
</div>
</div>
  
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .where({
        'author.name': {
          $regex: [params.author, 'i']
        }
      })
      .only(['title', 'description', 'img', 'slug', 'author', 'tags', 'updatedAt'])
      .without('body')
      .sortBy('createdAt', 'asc')
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
  head() {
  return {
    title: 'Contact ' + this.articles[0].author.name,
    meta: [
      // hid is used as unique identifier. Do not use `vmid` for it as it will not work
      {
        hid: 'description',
        name: 'description',
        content: this.articles[0].author.bio
      }
    ]
  }
}
}
</script>