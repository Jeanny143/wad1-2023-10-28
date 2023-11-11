<template>
    <h1 class="mb-4">Posts</h1>
    <v-textarea label="What's on your mind?"></v-textarea>
    <div class="posts">
    <p>{{ postText }}</p>
    <button @click="incrementLikes">
      <span>&#10084;</span> {{ likeCount }}
    </button>
  </div>
</template>


<script>
    export default {
  props: {
    postText: String,
    initialLikes: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      likeCount: this.initialLikes,
    };
  },
  methods: {
    incrementLikes() {
      this.likeCount += 1;
    },
  },
};
</script>


<style scoped>
.post {
  margin-bottom: 20px;
  border: 1px solid #ac1212;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

button {
  background: transparent;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
}
</style>
