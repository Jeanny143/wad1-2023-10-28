<template>
  <div>
    <h1 class="mb-4">Posts</h1>
    <v-textarea v-model="inputText" label="What's on your mind?" placeholder=" "></v-textarea>
    <v-btn @click="postText">Post</v-btn>

    <div v-if="postedText.length > 0" class="mt-4">
      <v-card v-for="(post, index) in postedText.slice().reverse()" :key="index" class="mb-2">
        <v-row align="center">
          
          <v-col>
            <v-icon>mdi-account-circle</v-icon> Jean Ayen
          </v-col>

          <v-col class="text-right">
            {{ formatDateTime(post.timestamp) }}
          </v-col>

        </v-row>
        <v-card-text>{{ post.message }}</v-card-text>
        <v-row align="center">
          <v-col>
            <v-icon @click="toggleReplyForm(post)" style="color: blue; cursor: pointer;">mdi-message-reply-text</v-icon>
            <v-btn @click="toggleReplyForm(post)">
              <v-icon>mdi-comment</v-icon>
              {{ getCommentCount(post) }}
            </v-btn>
          </v-col>
          <v-col class="text-right">
            <v-icon @click="incrementHeartCount(post)" style="color: red; background-color: #ff9999;">mdi-heart</v-icon> {{ post.heartCount }}
          </v-col>
        </v-row>

        <!-- Reply form -->
        <div v-show="post.showReplyForm" class="reply-form">
          <v-textarea v-model="post.replyText" label="Reply" placeholder=" "></v-textarea>
          <v-btn @click="postReply(post)">Reply</v-btn>
        </div>

        <!-- Display replies -->
        <v-divider class="mt-2"></v-divider>
        <div v-for="(comment, commentIndex) in post.comments" :key="commentIndex" class="mb-2">
          <v-row align="center">
            <v-col>
              <v-icon>mdi-account-circle</v-icon> {{ comment.author }}
            </v-col>
            <v-col class="text-right">
              {{ formatDateTime(comment.timestamp) }}
            </v-col>
          </v-row>
          <v-card-text>{{ comment.message }}</v-card-text>
          <v-row align="center">
            <v-col></v-col>
            <v-col class="text-right">
              <v-icon @click="incrementHeartCount(comment)" style="color: red; background-color: #ff9999;">mdi-heart</v-icon>
              {{ comment.heartCount }}
            </v-col>
          </v-row>
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'PostsPage',
  data() {
    return {
      inputText: '',
      postedText: [],
    };
  },
  methods: {
    postText() {
      const timestamp = new Date();
      this.postedText.push({ message: this.inputText, heartCount: 0, timestamp, comments: [], showReplyForm: false, replyText: '' });
      this.inputText = '';
    },
    incrementHeartCount(post) {

      post.heartCount++;
    },
    formatDateTime(timestamp) {
      const options = { year: 'numeric', month: '2-digit', day: '2-digit', hour: '2-digit', minute: '2-digit' };
      return new Date(timestamp).toLocaleString('en-US', options);

    },
    toggleReplyForm(post) {
      post.showReplyForm = !post.showReplyForm;
    },
    postReply(post) {
      const timestamp = new Date();
      const replyText = post.replyText;
      if (replyText.trim() !== '') {
        post.comments.push({
          author: 'Reply Author', // Replace with the actual author of the reply
          message: replyText,
          heartCount: 0,
          timestamp,
        });
        post.showReplyForm = false;
        post.replyText = '';
      }
    },
    getCommentCount(post) {
      return post.comments.length;
    },
  },
}
);
</script>

<style scoped>
.reply-form {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #e0e0e0;
}
</style>
