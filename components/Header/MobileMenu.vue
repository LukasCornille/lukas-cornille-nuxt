<template>
  <div :class="{ 'is-active': isActive }">
    <nav @click="$emit('click')">
      <nuxt-link to="/">Home</nuxt-link>
      <nuxt-link to="/blog" :class="{ 'nuxt-link-exact-active': isBlogActive }">Blog</nuxt-link>
      <nuxt-link to="/contact">Let's talk</nuxt-link>
    </nav>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    isActive: {
      type: Boolean,
      required: true,
    },
    isBlogActive: {
      type: Boolean,
      required: true,
    },
  },
});
</script>

<style scoped>
div {
  @apply fixed flex z-50 w-screen bg-gray-900;
  height: calc(100vh - 80px);
  margin-top: 80px;
  transform: translateX(-100%);
  transition: transform 150ms ease-in-out 0s;
}

.is-active {
  transform: translateX(0);
}

.nuxt-link-exact-active {
  @apply text-orange-700;
}

nav {
  @apply relative flex flex-col text-center justify-center text-4xl w-full;
  top: -40px;
}

nav a {
  @apply py-2;
}
</style>
