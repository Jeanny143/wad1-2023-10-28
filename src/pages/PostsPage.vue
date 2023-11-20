<template>
  <div>
    <h1 class="mb-4">Posts</h1>
    <v-textarea v-model="inputText" label="What's on your mind?" placeholder=" "></v-textarea>
    <v-btn @click="postText">Post</v-btn>

    <div v-if="postedText.length > 0" class="mt-4">
      <v-card v-for="(post, index) in postedText.slice().reverse()" :key="index" class="mb-2 post-card">
        <div class="post-header">
          <v-icon>mdi-account-circle</v-icon> Jean Ayen
          <div class="post-timestamp">{{ formatDateTime(post.timestamp) }}</div>
        </div>
        <v-card-text class="post-message">{{ post.message }}</v-card-text>
        <div class="post-actions">
       
          <v-btn @click="toggleReplyForm(post)" class="action-btn">
            <v-icon>mdi-comment</v-icon>
            {{ getCommentCount(post) }}
          </v-btn>
          <v-icon @click="toggleEditForm(post)" class="action-icon">mdi-pencil</v-icon>
          <v-icon @click="incrementHeartCount(post)" class="action-icon heart-icon">mdi-heart</v-icon>
          <span class="heart-count">{{ post.heartCount }}</span>
          <v-icon @click="deletePost(index)" class="action-icon delete-icon">mdi-delete</v-icon>
        </div>

        <!-- Edit form for post -->
        <div v-show="post.showEditForm" class="edit-form">
          <v-textarea v-model="post.editedText" label="Edit Post" placeholder=" "></v-textarea>
          <v-btn @click="updatePost(post)" class="update-btn">Update</v-btn>
        </div>

        <!-- Reply form -->
        <div v-show="post.showReplyForm" class="reply-form">
          <v-textarea v-model="post.replyText" label="Reply" placeholder=" "></v-textarea>
          <v-btn @click="postReply(post)" class="reply-btn">Reply</v-btn>
        </div>

        <!-- Display replies -->
        <v-divider class="mt-2"></v-divider>
        <div v-for="(comment, commentIndex) in post.comments" :key="commentIndex" class="mb-2 comment">
          <div class="comment-header">
            <v-icon>mdi-account-circle</v-icon> {{ comment.author }}
            <div class="comment-timestamp">{{ formatDateTime(comment.timestamp) }}</div>
          </div>
          <v-card-text>{{ comment.message }}</v-card-text>
          <div class="comment-actions">
            <v-icon @click="incrementHeartCount(comment)" class="action-icon heart-icon">mdi-heart</v-icon>
            {{ comment.heartCount }}
            <v-icon @click="toggleEditForm(comment)" class="action-icon">mdi-pencil</v-icon>
            <v-icon @click="deleteComment(post, commentIndex)" class="action-icon delete-icon">mdi-delete</v-icon>
          </div>

          <!-- Edit form for comment -->
          <div v-show="comment.showEditForm" class="edit-form">
            <v-textarea v-model="comment.editedText" label="Edit Comment" placeholder=" "></v-textarea>
            <v-btn @click="updateComment(post, commentIndex)" class="update-btn">Update</v-btn>
          </div>
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
      this.postedText.push({
        message: this.inputText,
        heartCount: 0,
        timestamp,
        comments: [],
        showReplyForm: false,
        replyText: '',
        showEditForm: false,
        editedText: this.inputText,
      });
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
          showEditForm: false,
          editedText: replyText,
        });
        post.showReplyForm = false;
        post.replyText = '';
      }
    },
    toggleEditForm(item) {
      item.showEditForm = !item.showEditForm;
      item.editedText = item.message;
    },
    updatePost(post) {
      post.message = post.editedText;
      post.showEditForm = false;
    },
    updateComment(post, commentIndex) {
      const comment = post.comments[commentIndex];
      comment.message = comment.editedText;
      comment.showEditForm = false;
    },
    deletePost(index) {
      this.postedText.splice(index, 1);
    },
    deleteComment(post, commentIndex) {
      post.comments.splice(commentIndex, 1);
    },
    getCommentCount(post) {
      return post.comments.length;
    },
  },
});
</script>

<style scoped>
.post-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
}

.post-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.post-timestamp,
.comment-timestamp {
  color: #888;
  font-size: 0.8rem;
}

.post-message {
  margin-bottom: 12px;
}

.post-actions,
.comment-actions {
  display: flex;
  align-items: center;
  color: #555;
}

.action-icon {
  margin-right: 8px;
  cursor: pointer;
}

.action-btn {
  text-transform: none;
}

.heart-icon {
  color: red;
}

.heart-count {
  margin-right: 12px;
}

.delete-icon {
  color: red;
}

.edit-form,
.reply-form {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #080404;
  background-color: #130a0a;
}

.reply-btn,
.update-btn {
  margin-top: 8px;
}

.comment {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 12px;
}

.comment-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}
</style>