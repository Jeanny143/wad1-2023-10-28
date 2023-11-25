<template>
  <div>
    <h1 class="mb-4 text-primary">My Awesome Posts</h1>

    <div class="row">
      <div class="col-md-6">
        <v-textarea v-model="inputText" label="Share your thoughts" placeholder="What's on your mind?"></v-textarea>
      </div>
      <div class="col-md-6 text-right">
        <v-btn @click="postText" class="btn-primary mt-3">Post</v-btn>
      </div>
    </div>

    <div v-if="postedText.length > 0" class="mt-4">
      <v-card v-for="(post, index) in postedText.slice().reverse()" :key="index" class="mb-4 post-card">
        <div class="post-header">
          <div>
            <v-icon>mdi-account-circle</v-icon> Jean Ayen
          </div>
          <div class="post-timestamp">{{ formatDateTime(post.timestamp) }}</div>
        </div>
        <v-card-text class="post-message">{{ post.message }}</v-card-text>
        <div class="post-actions">
          <v-btn @click="toggleReplyForm(post)" class="btn-icon btn-secondary">
            <v-icon>mdi-comment</v-icon>
            {{ getCommentCount(post) }}
          </v-btn>
          <v-icon @click="toggleEditForm(post)" class="action-icon edit-icon">mdi-pencil</v-icon>
          <v-icon @click="incrementHeartCount(post)" class="action-icon heart-icon" :style="{ backgroundColor: '#e74c3c' }">mdi-heart</v-icon>
          <span class="heart-count">{{ post.heartCount }}</span>
          <v-icon @click="confirmDelete(post)" class="action-icon delete-icon">mdi-delete</v-icon>
        </div>
        <!-- Edit form for post -->
        <div v-show="post.showEditForm" class="edit-form">
          <v-textarea v-model="post.editedText" label="Edit Post" placeholder="Edit your post"></v-textarea>
          <v-btn @click="confirmEdit(post)" class="btn-primary">Update</v-btn>
        </div>

        <!-- Reply form -->
        <div v-show="post.showReplyForm" class="reply-form">
          <v-textarea v-model="post.replyText" label="Reply" placeholder="Reply to this post"></v-textarea>
          <v-btn @click="postReply(post)" class="btn-primary">Reply</v-btn>
        </div>

        <!-- Display replies -->
        <v-divider class="mt-2"></v-divider>
        <div v-for="(comment, commentIndex) in post.comments" :key="commentIndex" class="mb-3 comment">
          <div class="comment-header">
            <div>
              <v-icon>mdi-account-circle</v-icon> {{ comment.author }}
            </div>
            <div class="comment-timestamp">{{ formatDateTime(comment.timestamp) }}</div>
          </div>
          <v-card-text>{{ comment.message }}</v-card-text>
          <div class="comment-actions">
            <v-icon @click="incrementHeartCount(comment)" class="action-icon heart-icon" :style="{ backgroundColor: '#e74c3c' }">mdi-heart</v-icon>
            <span class="heart-count">{{ comment.heartCount }}</span>
            <v-icon @click="toggleEditForm(comment)" class="action-icon edit-icon" :style="{ color: '#2ecc71' }">mdi-pencil</v-icon>
            <v-icon @click="confirmDelete({ post, commentIndex }, true)" class="action-icon delete-icon">mdi-delete</v-icon>
          </div>
          <!-- Edit form for comment -->
          <div v-show="comment.showEditForm" class="edit-form">
            <v-textarea v-model="comment.editedText" label="Edit Comment" placeholder="Edit your comment"></v-textarea>
            <v-btn @click="updateComment(post, commentIndex)" class="btn-primary">Update</v-btn>
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
  const confirmed = window.confirm('Are you sure you want to edit this comment?');
  if (confirmed) {
    comment.message = comment.editedText;
    comment.showEditForm = false;
  }
},

    confirmEdit(post) {
      const confirmed = window.confirm('Are you sure you want to edit this post?');
      if (confirmed) {
        this.updatePost(post);
      }
    },
    confirmDelete(item, isComment = false) {
      const confirmed = window.confirm('Are you sure you want to delete this ' + (isComment ? 'comment' : 'post') + '?');
      if (confirmed) {
        if (isComment) {
          // Delete comment
          const postIndex = this.postedText.indexOf(item.post);
          if (postIndex !== -1) {
            this.postedText[postIndex].comments.splice(item.commentIndex, 1);
          }
        } else {
          // Delete post
          const index = this.postedText.indexOf(item);
          if (index !== -1) {
            this.postedText.splice(index, 1);
          }
        }
      }
    },
    deletePost(post) {
      const index = this.postedText.indexOf(post);
      if (index !== -1) {
        this.postedText.splice(index, 1);
      }
    },
    deleteComment(post, commentIndex) {
      this.confirmDelete({ post, commentIndex }, true);
    },
    getCommentCount(post) {
      return post.comments.length;
    },
  },
});
</script>

<style scoped>
/* New or modified styles */
.text-primary {
  color: #3498db;
}

.btn-primary {
  color: #fff;
  background-color: #3498db;
}

.btn-secondary {
  color: #fff;
  background-color: #2ecc71;
}

.post-card {
  border: 1px solid #806363;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
  background-color: #746868;
  box-shadow: 0 0 10px rgba(114, 83, 83, 0.1);
}

.post-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.post-timestamp,
.comment-timestamp {
  color: #555; /* Set the color to a dark shade */
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
.edit-icon {
  color: #2ecc71; /* Green color for the edit icon */
  cursor: pointer;
}
.heart-count {
  background-color: #e74c3c; /* Match the heart icon color */
  color: #fff;
  padding: 2px 6px;
  border-radius: 4px;
  margin-right: 8px;
}

.delete-icon {
  color: red;
}

.edit-form,
.reply-form {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #291e1e;
  background-color: #634a4a;
}

.reply-btn,
.update-btn {
  margin-top: 8px;
}

.comment {
  border: 1px solid #241a1a;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 12px;
  background-color: #9e8585;
  box-shadow: 0 0 5px rgba(65, 53, 53, 0.1);
}

.comment-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}
</style>
