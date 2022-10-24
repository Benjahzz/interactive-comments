<script setup>
import dataJson from '/data.json'
import Comment from './components/Comment.vue'
import { reactive } from 'vue';
import Reply from './components/Reply.vue';
const comments = reactive(dataJson.comments)
const user = dataJson.currentUser

const handleDeleteComment = (commentId) => {
  const index = comments.findIndex((comment) => comment.id === commentId)
  comments.splice(index, 1)
}
const handleDeleteReply = (commentId, replyId) => {
  comments.value = comments.map((comment) => {
    if (comment.id === commentId) {
      comment.replies = comment.replies.filter((reply) => reply.id !== replyId)
    }
    return comment
  })
}

const handleAction = (commentId, replyId) => {
  if (commentId && replyId) {
    handleDeleteReply(commentId, replyId)
  } else if (commentId) {
    handleDeleteComment(commentId)
  }
}
const handleAddComment = () => {
  const newComment = {
    "id": crypto.randomUUID(),
    "content": textComment.text,
    "createdAt": "0 minutes ago",
    "score": 0,
    "user": user,
    "replies": []
  }
  comments.push(newComment)
}
const textComment = reactive({
  text: '',
  error: false,
  reply: null,
})
const handleAddReply = () => {
  const indexComents = comments.findIndex((comment) => comment.id === textComment.reply.id)
  if (indexComents !== -1) {
    const newReply = {
      "id": crypto.randomUUID(),
      "content": textComment.text,
      "createdAt": "0 minutes ago",
      "score": 0,
      "user": user,
    }
    comments[indexComents].replies.push(newReply)
  } else {
    comments.value = comments.map((comment) => {
      const indexReplies = comment.replies.findIndex((reply) => reply.id === textComment.reply.id)
      if (indexReplies !== -1) {
        const newReply = {
          "id": crypto.randomUUID(),
          "content": textComment.text,
          "createdAt": "0 minutes ago",
          "score": 0,
          "user": user,
        }
        comment.replies.splice(indexReplies + 1, 0, newReply)
      }
      return comment
    })
  }
}
const addReplies = (comment) => {
  textComment.reply = comment
}

</script>

<template>
  
  <main class="container">
    <div class="container-comments">

      <TransitionGroup name="list" tag="div" class="list-item">
        <Comment v-for="comment in comments" :key="comment.id" :comment="comment" :admin="user"
          @handleAction="handleAction" @handleReplies="addReplies">
          <template #replies v-if="comment.replies.length > 0">
            <div class="replies-list">
              <div class="replies__line" :key="comment.id">
                <div class="line"></div>
              </div>
              <TransitionGroup name="list" tag="div" class="container-replies" appear>

                <Reply v-for="reply in comment.replies" :key="reply.id" :reply="reply" :comment="comment" :admin="user"
                  @handleAction="handleAction" @handleReplies="addReplies" />
              </TransitionGroup>

            </div>


          </template>
        </Comment>


      </TransitionGroup>



    </div>
    <div class="container-comentar">
      <div class="comentar">
        <div class="img">
          <img :src="user.image.png" alt="" class="comentar__user" />
        </div>
        <Transition name="bounce">
          <div v-if="textComment.reply !== null" class="reply-user">
            To: <span>{{textComment.reply.user.username}}</span> <i class="fa-solid fa-xmark"
              @click="textComment.reply = null"></i>
          </div>
        </Transition>

        <textarea placeholder="Add a comment..." name="textComment" id="" cols="30" rows="10" class="comentar__textarea"
          v-model="textComment.text">

          </textarea>
        <button class="btn btn-comentar"
          @click="textComment.reply !== null ? handleAddReply() : handleAddComment()">Send</button>

      </div>

    </div>
  </main>

</template>


<style>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
}

.bounce-enter-active {
  animation: bounce-in 0.3s;
}

.bounce-leave-active {
  animation: bounce-in 0.3s reverse;
}

@keyframes bounce-in {
  0% {
    transform: scale(0) translate(-50%);
  }

  50% {
    transform: scale(1.2) translate(-50%);
  }

  100% {
    transform: scale(1) translate(-50%);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>