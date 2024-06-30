<script setup>
  import { defineProps } from 'vue';
  const { user, currentComp } = defineProps({
    user: {
      type: String,
      required: true,
    },
    currentComp: {
      type: String,
      required: true,
    },
  });

  const emit = defineEmits(['changeComponent']);

</script>

<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
    <div class="container">
      <a class="navbar-brand" href="#">BAT Blog</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a :class="{ 'nav-link': true, active: currentComp == 'posts' }" aria-current="page" href="#" @click="emit('changeComponent', 'posts')">Posts</a>
          </li>
          <li class="nav-item">
            <a :class="{ 'nav-link': true, active: currentComp == 'createPost' }" href="#" @click="emit('changeComponent', 'createPost')">CreatePost</a>
          </li>
        </ul>
        <ul class="navbar-nav mb-2 mb-lg-0">
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              {{ user }}
            </a>
            <ul class="dropdown-menu dropdown-menu-end">
              <li><a class="dropdown-item" href="#">Profile</a></li>
              <li><a class="dropdown-item" href="#">Logout</a></li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>