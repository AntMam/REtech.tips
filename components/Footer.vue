<template>
    
  <div class="mx-auto max-w-6xl p-4 lg:px-16 text-center">
    <hr class="mt-4">
    <div class="mx-auto py-4">
      <div class="pb-4">
        Join the newsletter to be notified when we post new content.
      </div>
      <div class="mx-auto w-full md:w-1/2">
      <form name="contact" action="/thank-you" method="POST" data-netlify="true">
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 items-center">
          <input type="hidden" name="form-name" value="contact" />
          <input type="email" v-model="email" name="email" placeholder="Enter your email address" class="rounded mc text-center border-2 border-gray-500" required>
          <input type="submit" name="subscribe" value="Subscribe" class="mc-btn rounded px-2 py-1 w-full" :disabled="!email">
        </div>
      </form> 
      </div>
    </div>
    <hr class="mt-4">
  </div>

</template>

<script>
export default {
  data() {
    return {
      email: '',
    };
  }
}
</script>