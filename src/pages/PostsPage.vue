<!-- UserPost.vue -->
<template>
  <div>
    <textarea v-model="newPostText" placeholder="Type your post here..."></textarea>
    <button @click="addPost">Post</button>

    <Post v-for="(post, index) in userPosts" :key="index" :postText="post.text" :initialLikes="post.likes" @like="incrementLikes(index)" />
  </div>
</template>

<script>
import Post from "./Post.vue";

export default {
  components: {
    Post,
  },
  data() {
    return {
      newPostText: "",
      userPosts: [],
    };
  },
  methods: {
    addPost() {
      if (this.newPostText.trim() !== "") {
        this.userPosts.push({
          text: this.newPostText,
          likes: 0,
        });
        this.newPostText = "";
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
