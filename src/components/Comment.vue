<script setup>
import { reactive, ref } from 'vue'
import CommentDelete from './CommentDelete.vue'
import Rating from './Rating.vue'
const props = defineProps({
    comment: {
        type: Object,
    },
    index: {
        type: Number,
    },
    admin: {
        type: Object,
    },
})
const emit = defineEmits(['handleAction', 'handleReplies'])
const showEdit = ref(false)
const deleteModal = reactive({
    showed: false,
})
const showDeleteModal = () => {
    deleteModal.showed = true
    document.body.style.overflow = 'hidden'
}
const handleDeleteModal = (obj) => {
    if (obj.cancel) {
        deleteModal.showed = false
        deleteModal.comment = null
        return
    }
    emit('handleAction', props.comment.id)
    deleteModal.showed = false
}
const handleReplies = (comment) => {
    emit('handleReplies', comment)
}
</script>
<template>
    <div class="comment-container">
        <div class="comment">
            <Rating :comment="comment" />
            <div class="comment__content">
                <div class="comment__content__header">
                    <div class="comment__content__header__user">
                        <img :src="comment.user.image.png" alt="" class="user__image" />
                        <div class="user__name user__name--admin" v-if="admin.username === comment.user.username">
                            {{comment.user.username}}
                            <div class="user-admin__tooltip">
                                You
                            </div>
                        </div>

                        <div class="user__name" v-else>
                            {{comment.user.username}}
                        </div>
                        <div class="user__date">{{comment.createdAt}}</div>
                    </div>
                    <div class="comment__content__header__actions" v-if="admin.username === comment.user.username">
                        <div class="action action-eliminar" @click="showDeleteModal()">
                            <i class="fa-solid fa-trash"></i>
                            Delete
                        </div>
                        <div class="action action-editar" @click="showEdit = !showEdit">
                            <i class="fa-solid fa-pen"></i>
                            Editar
                        </div>
                    </div>
                    <div class="comment__content__header__actions" v-else>
                        <div class="action action-reply" @click="handleReplies(comment)">
                            <i class="fa-solid fa-reply"></i>
                            Reply
                        </div>
                    </div>
                </div>
                <div class="comment__body" v-if="!showEdit">
                    {{comment.content}}
                </div>
                <div class="comment__body" v-else>
                    <textarea class="comment__body__textarea" v-model="comment.content"></textarea>
                    
                </div>
            </div>

        </div>
        <slot name="replies"></slot>
        <div class="wrapper-modal" v-if="deleteModal.showed">
        </div>
        <Transition name="bounce">
            <CommentDelete :modal="deleteModal" @handleAction="handleDeleteModal" v-if="deleteModal.showed" />
        </Transition>

    </div>



</template>
<style>
.bounce-enter-active {
    animation: bounce-in 0.5s;
}

.bounce-leave-active {
    animation: bounce-in 0.5s reverse;
}

@keyframes bounce-in {
    0% {
        transform: scale(0) translate(-50%);
    }

    50% {
        transform: scale(1.25) translate(-50%);
    }

    100% {
        transform: scale(1) translate(-50%);
    }
}
</style>