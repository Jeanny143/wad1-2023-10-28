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

        </v-row>
        <v-card-text>{{ post.message }}</v-card-text>
        <v-row align="center">
          <v-col></v-col>
          <v-col class="text-right">
            <v-icon @click="incrementHeartCount(post)" style="color: red; background-color: #ff9999;">mdi-heart</v-icon> {{ post.heartCount }}
          </v-col>
        </v-row>
        <v-divider class="mt-2"></v-divider>
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
    postsText() {
      const currentDate = new Date();
      const formattedDate = `${currentDate.toLocaleDateString()} ${currentDate.toLocaleTimeString()}`;

      this.postedText.push({ message: this.inputText, heartCount: 0, createdAt: formattedDate });
      this.inputText = '';
    },
    incrementHeartCount(post) {
      posts.heartCount++;
    },
  },
});
</script>

   
   <style scoped>
   /* Add your styles here if needed */
   /* Add your component styles here */
.post {
  border: 1px solid #7c1a92;
  padding: 10px;
  margin: 10px;
}

button {
  background: green;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  float: right;
}

   </style>