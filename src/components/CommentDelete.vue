<script setup>

import { defineComponent, ref, defineProps } from 'vue'
const props = defineProps({
    modal: {
        type: Object,
    },
})
const emit = defineEmits(['handleAction'])
const cancelAction = () => {
    emit('handleAction', { cancel: true })
    document.body.style.overflow = 'auto'

}
const confirmAction = () => {
    if (props.modal.reply) {
        emit('handleAction', { replyId: props.modal.comment })
    } else {
        emit('handleAction', { cancel: false })
    }
    document.body.style.overflow = 'auto'

}

</script>

<template>
    <div class="modal modal-delete">
        <h1 class="title-modal">Delete comment</h1>
        <p class="text-modal">Are you sure you want to delete this comment? This will remove the comment?
            and can't be undone.</p>
        <div class="modal__actions">
            <button class="btn-modal btn-cancel" @click="cancelAction">No, Cancel</button>
            <button class="btn-modal btn-delete" @click="confirmAction">Yes, Delete</button>

        </div>
    </div>
</template>

<style>

</style>
