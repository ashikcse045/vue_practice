<script setup>
import { defineEmits, ref } from 'vue';
const fromData = ref({
  title: '',
  content: '',
})
const emit = defineEmits(['createPost']);
const createPost = () => {
  emit('createPost', {...fromData.value});
  fromData.value = {
    title: '',
    content: '',
  }
}

</script>

<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Create a New Post</h5>
            <form @submit.prevent="createPost">
              <div class="mb-3">
                <label for="title" class="form-label">Title</label>
                <input type="text" class="form-control" id="title" placeholder="Enter the title"
                  v-model="fromData.title">
              </div>
              <div class="mb-3">
                <label for="content" class="form-label">Content</label>
                <textarea class="form-control" id="content" rows="3" placeholder="Enter the content"
                  v-model="fromData.content"></textarea>
              </div>
              <button type="submit" class="btn btn-success">Submit</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>