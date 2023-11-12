<template>
  <h1 class="mb-4">Posts</h1>
  <v-textarea label="What's on your mind?"></v-textarea>

  <div>
    <button @click="addPost">Post</button>

    <Posts v-for="(post, index) in userPosts" :key="index" :postText="post.text" :initialLikes="post.likes" @like="incrementLikes(index)" />
  </div>
</template>

<script>
    import { defineComponent } from 'vue';

    export default defineComponent({
        name: "HomePage",
    })
</script>


<style scoped>

</style>