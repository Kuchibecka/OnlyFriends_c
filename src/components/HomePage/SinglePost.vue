<template>
  <q-item-section avatar top>
    <q-avatar size="xl">
      <img src="https://cdn.quasar.dev/img/avatar2.jpg"/>
    </q-avatar>
  </q-item-section>
  <q-item-section>
    <q-item-label class="text-subtitle1">
      <strong>Author's name</strong>
      <span class="text-grey-7 q-ml-xs"> @author_tag </span>
      <br class="lt-md"/>
      &bull; {{ howLongAgo(modelValue.date) }}
    </q-item-label>
    <q-item-label class="post-text text-body1 q-pt-xs">
      {{ modelValue.text }}
    </q-item-label>
    <div class="row justify-between q-mt-md">
      <q-btn
        @click="toggleLike(modelValue)"
        flat
        round
        :color="modelValue.liked ? 'red-5' : 'grey'"
        size="sm"
        :icon="modelValue.liked ? 'fas fa-heart' : 'far fa-heart'"
      />
      <q-btn flat round color="grey" size="sm" icon="reply"/>
      <q-btn flat round color="grey" size="sm" icon="comment"/>
      <q-btn
        flat
        round
        color="grey"
        size="sm"
        icon="delete"
        @click="deletePost(modelValue)"
      />
    </div>
  </q-item-section>
</template>

<script setup>
import {formatDistance} from "date-fns";
import {deleteDoc, doc, updateDoc} from "firebase/firestore";
import db from "boot/firebase";

defineProps({
  modelValue: Array,
});

function howLongAgo(date) {
  // docs: https://date-fns.org/v2.29.3/docs/Getting-Started
  return formatDistance(Number(date), new Date(), { addSuffix: true });
}

async function deletePost(post) {
  await deleteDoc(doc(db, "posts", post.id));
}

async function toggleLike(post) {
  post.liked = !post.liked;

  await updateDoc(doc(db, "posts", post.id), {
    liked: post.liked,
  });
}
</script>

<style scoped>

</style>
