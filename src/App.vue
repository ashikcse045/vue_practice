<script setup>
import { ref, watch } from 'vue';
import moment from 'moment';
import Posts from './components/Posts.vue';
import CreatePost from './components/CreatePost.vue';
import NavItem from './components/NavItem.vue';

const posts = ref([
  {
    id: 1,
    title: 'Post 1',
    content: 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00'
  },
  {
    id: 2,
    title: 'Post 2',
    content: 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00'
  },
  {
    id: 3,
    title: 'Post 3',
    content: 'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00'
  }
]);

const user = ref('Ashik Sarker');
const changeUserName = (name) => {
  user.value = name;
}

const increaseLike = i => {
  posts.value[i].likes += 1;
}
const deletePost = i => {
  posts.value.splice(i, 1);
}

const fromData = ref({
  title: '',
  content: '',
})
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

const createPost = () => {
  const isValid = checkValidity({ ...fromData.value });
  if (!isValid.result) {
    alert(isValid.message);
    return false;
  }
  posts.value.push({
    id: posts.value.length + 1,
    title: fromData.value.title,
    content: fromData.value.content,
    likes: 0,
    comments: [],
    date: moment().format('YYYY-MM-DD HH:mm:ss'),
  });
  emptyFromData(fromData.value);
}

const commentData = ref({});
const makeComment = i => {
  const isValid = checkValidity({ comment: commentData.value[i] });
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

const isCommentVisable = ref({});
const toggleCommentVisibility = i => {
  isCommentVisable.value[i] = !isCommentVisable.value[i];
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
</script>

<template>

  <NavItem :user="user" @changeUserName="changeUserName" />
  <CreatePost :fromData="fromData" @createPost="createPost" />
  <Posts 
    :posts="posts" 
    :isCommentVisable="isCommentVisable" 
    :commentData="commentData"
    @increaseLike="increaseLike" 
    @deletePost="deletePost"
    @toggleCommentVisibility="toggleCommentVisibility"
    @makeComment="makeComment"
    @deleteComment="deleteComment"
  />

</template>

<style scoped>

</style>
