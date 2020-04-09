<template>
  <div>
    <posts ref="posts" />
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import Posts from '~/components/Posts.vue'
export default {
 components: {
   Posts
 },
 methods: {
   openModal () {
     this.$refs.posts.openModal()
   }
 }
}
</script>

<style>
.post {
 top: 12px;
 right: 10px;
}
</style>
