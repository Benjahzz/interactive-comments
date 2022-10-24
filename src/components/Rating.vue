<script setup>
import { defineComponent, ref, defineProps } from 'vue'
const props = defineProps({
    comment: {
        type: Object,
    },
})
const voteAdd = ref(false)
const voteSub = ref(false)
const initialRating = props.comment.score
const handleVoteAdd = () => {
    if( voteAdd.value === false && voteSub.value === false){
        voteAdd.value = true
        voteSub.value = false
        props.comment.score = initialRating + 1

    } else if (voteAdd.value === true && voteSub.value === false){
        voteAdd.value = false
        voteSub.value = false
        props.comment.score = initialRating
    } else if (voteAdd.value === false && voteSub.value === true){
        voteAdd.value = true
        voteSub.value = false
        props.comment.score = initialRating + 1
    }

}
const handleVoteSub = () => {
    
    if( voteAdd.value === false && voteSub.value === false){
        voteAdd.value = false
        voteSub.value = true
        props.comment.score = initialRating - 1

    } else if (voteAdd.value === true && voteSub.value === false){
        voteAdd.value = false
        voteSub.value = true
        props.comment.score = initialRating - 1
    } else if (voteAdd.value === false && voteSub.value === true){
        voteAdd.value = false
        voteSub.value = false
        props.comment.score = initialRating
    }
}
</script>

<template>
    <div class="comment__rating">
        <div class="comment__rating__up" :class="voteAdd ? 'rating--active' : ''" @click="upRating">
            <i class="fa-solid fa-plus" @click="handleVoteAdd"></i>
        </div>
        <div class="comment__rating__value">
            {{ props.comment.score }}
        </div>
        <div class="comment__rating__down" :class="voteSub ? 'rating--active' : ''" @click="handleVoteSub">
            <i class="fa-solid fa-minus"></i>
        </div>
    </div>
</template>

