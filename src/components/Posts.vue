<script setup>
import { defineProps, defineEmits, computed, ref, inject, onBeforeMount, onMounted, onBeforeUnmount, onUnmounted } from 'vue';
import moment from 'moment';
import Comments from './Comments.vue';
const { isCommentVisable } = defineProps([
  'isCommentVisable',
]);

const posts = inject('posts')

const reversedPosts = computed(() => {
  return [...posts.value].reverse();
})

const emit = defineEmits([
  'increaseLike',
  'deletePost',
  'toggleCommentVisibility',
  'makeComment',
  'deleteComment'
]);

const makeComment = index => {
  emit('makeComment', index, commentData);
}
const deleteComment = (index, commentIndex) => {
  emit('deleteComment', index, commentIndex);
}

const commentData = ref({});


// VUE lifecycle methods
onBeforeMount( () => {
  console.log('posts: before mount');
} );

onMounted(() => {
  console.log('posts: mounted');
});

onBeforeUnmount(() => {
  console.log('posts: before unmounted');
});

onUnmounted(() => {
  console.log('posts: unmounted')
})

</script>

<template>
  <div class="posts mt-4">
    <div class="container">
      <div class="nav nav-tabs mb-4">
        <h3>Posts</h3>
      </div>

      <div class="row">
        <div class="col-md-4" v-for="post, i in reversedPosts" :key="post.id">
          <div class="card mb-4">
            <div class="card-body">
              <h5 class="card-title">{{ post.title }}</h5>
              <h6 class="card-subtitle mb-2 text-body-secondary">{{ moment(post.date).fromNow() }}</h6>
              <p class="card-text">{{ post.content }}</p>
              <p class="card-text">
                <span :style="{ color: post.likes >= 10 ? 'green' : '' }">
                  <small v-if="post.likes === 0">No likes</small>
                  <small v-else-if="post.likes === 1">{{ post.likes }} like</small>
                  <small v-else>{{ post.likes }} likes</small>,
                </span>
                <span class="cursor-pointer" @click="emit('toggleCommentVisibility', reversedPosts.length - (i + 1))"
                  :style="{ color: post.comments.length >= 3 ? 'green' : '' }">
                  <small v-if="post.comments.length > 1">{{
          post.comments.length }} comments</small>
                  <small v-else-if="post.comments.length < 1">No comments</small>
                  <small v-else>{{ post.comments.length }}
                    comment</small>
                </span>
              </p>
              <Comments :index="i" :mainIndex="reversedPosts.length - (i + 1)"
                @makeComment="makeComment" @deleteComment="deleteComment"
                v-model="commentData[reversedPosts.length - (i + 1)]" />
              <button class="btn btn-sm btn-primary"
                @click="emit('increaseLike', reversedPosts.length - (i + 1))">Like</button>
              <button class="btn btn-sm btn-danger float-end"
                @click="emit('deletePost', reversedPosts.length - (i + 1))"><i class="bi bi-trash"></i></button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}
</style>