<script setup>
import { defineProps, defineEmits, computed, defineModel, inject } from 'vue';
import moment from 'moment';
const { index, mainIndex } = defineProps([
    'index',
    'mainIndex',
]);

const posts = inject('posts');
const post = posts.value[mainIndex]
const comments = post.comments

const reversedComments = computed(() => {
    return [...comments].reverse();
})

const emit = defineEmits([
    'makeComment',
    'deleteComment',
])

const commentFromData = defineModel();

</script>

<template>
    <div class="comments mb-3">
        <div class="comments-input d-flex mb-3">
            <input type="text" class="form-control form-control-sm me-2" placeholder="Write Comment"
                v-model="commentFromData">
            <button class="btn btn-sm btn-success" @click="emit('makeComment', mainIndex)"><i class="bi bi-send"></i></button>
        </div>

        <template v-if="post.isCommentVisable">
            <div class="comment mb-3 ms-3" v-for="comment, commentIndex in reversedComments" :key="comment.id">
                <h6 class="card-title small">{{ comment.user }} <span class="text-danger float-end cursor-pointer"
                        @click="emit('deleteComment', mainIndex, reversedComments.length - (commentIndex+1))">X</span></h6>
                <p class="card-subtitle mb-1 text-body-secondary small">{{ moment(comment.time).fromNow() }}</p>
                <p class="card-text small">{{ comment.text }}</p>
                <hr class="my-2 very-low-opacity" v-if="commentIndex < comments.length - 1">
            </div>
        </template>

    </div>
</template>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}

.very-low-opacity {
  opacity: 0.1;
}
</style>