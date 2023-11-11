<!-- UserPost.vue -->
<template>
  <div>
    <textarea v-model="newPostsText" placeholder="Type your post here..."></textarea>
    <button @click="addPosts">Post</button>

    <Post v-for="(posts, index) in userPosts" :key="index" :postsText="posts.text" :initialLikes="posts.likes" @like="incrementLikes(index)" />
  </div>
</template>

<script>
import PostsPage      from "../pages/PostsPage.vue";

export default {
  components: {
    PostsPage,
  },
  data() {
    return {
      newPostsText: "",
      userPosts: [],
    };
  },
  methods: {
    addPost() {
      if (this.newPostsText.trim() !== "") {
        this.userPosts.push({
          text: this.newPostsText,
          likes: 0,
        });
        this.newPostsText = "";
      }
    },
    incrementLikes(index) {
      this.$set(this.userPosts, index, {
        ...this.userPosts[index],
        likes: this.userPosts[index].likes + 1,
      });
    },
  },
};
</script>

<style scoped>
textarea {
  width: 100%;
  margin-bottom: 10px;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
}
</style>
