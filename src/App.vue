<script setup>
import { ref, watch, provide } from 'vue';
import moment from 'moment';
import Posts from './components/Posts.vue';
import CreatePost from './components/CreatePost.vue';
import NavItem from './components/NavItem.vue';
import LayoutContent from './components/LayoutContent.vue';

const posts = ref([
  {
    id: 1,
    title: 'Post 1',
    content: 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00',
    isCommentVisable: false,
  },
  {
    id: 2,
    title: 'Post 2',
    content: 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00',
    isCommentVisable: false,
  },
  {
    id: 3,
    title: 'Post 3',
    content: 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00',
    isCommentVisable: false,
  }
]);

const user = ref('Ashik Sarker');

const increaseLike = i => {
  posts.value[i].likes += 1;
}
const deletePost = i => {
  posts.value.splice(i, 1);
}

const emptyFromData = obj => {
  for (const key in obj) {
    obj[key] = '';
  }
}

const checkValidity = obj => {
  for (let key in obj) {
    if (!obj[key]) {
      return {
        result: false,
        message: `${key} is required`,
      };
    }
  }
  return {
    result: true,
    message: `Valid`,
  };
}

const createPost = (fromData) => {
  console.log('this function called');
  const isValid = checkValidity({ ...fromData });
  if (!isValid.result) {
    alert(isValid.message);
    return false;
  }
  posts.value.push({ 
    id: posts.value.length + 1,
    title: fromData.title,
    content: fromData.content,
    likes: 0,
    comments: [],
    date: moment().format('YYYY-MM-DD HH:mm:ss'),
    isCommentVisable: false,
  });
}

const makeComment = (i, commentData) => {

  const isValid = checkValidity({ comment: commentData });
  if (!isValid.result) {
    alert(isValid.message);
    return false;
  }
  posts.value[i].comments.push({
    id: posts.value[i].comments.length + 1,
    text: commentData.value[i],
    user: user.value,
    time: moment().format('YYYY-MM-DD HH:mm:ss'),
  });
  commentData.value[i] = '';
}

const deleteComment = (i, commentIndex) => {
  posts.value[i].comments.splice(commentIndex, 1);
}

const toggleCommentVisibility = i => {
  posts.value[i].isCommentVisable = !posts.value[i].isCommentVisable;
}

const currentComponent = ref('posts');
const changeComponent = component => {
  currentComponent.value = component;
}

watch(() => posts.value.length,
(newVal, oldVal) => {
  if(newVal > oldVal) {
    alert('new post created');
  }
});

watch(() => posts.value, () => {
  posts.value.forEach(e => {
    if(e.likes >= 10 && !e.likeAlert) {
      e.likeAlert = true;
      alert('Congratulations! A post has reached 10 likes');
    }
    if(e.comments.length >= 3 && !e.commentAlert) {
      e.commentAlert = true;
      alert('Congratulations! A post has reached 3 comments');
    }
  })
}, { deep: true })

provide('posts', posts)
</script>

<template>
  <LayoutContent :user="user" @changeComponent="changeComponent" :currentComp="currentComponent">
    <CreatePost v-if="currentComponent == 'createPost'" @createPost="createPost" />
    <Posts v-if="currentComponent == 'posts'"
      @increaseLike="increaseLike" 
      @deletePost="deletePost"
      @toggleCommentVisibility="toggleCommentVisibility"
      @makeComment="makeComment"
      @deleteComment="deleteComment"
    />
  </LayoutContent>
</template>

<style scoped>

</style>
