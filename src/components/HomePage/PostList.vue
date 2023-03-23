<template>
  <div>
    <q-list separator>
      <transition-group
        appear
        enter-active-class="animated fadeInDown"
        leave-active-class="animated fadeOutDown"
      >
        <q-item class="q-py-md" v-for="post in modelValue" :key="post.id">
          <q-item-section avatar top>
            <q-avatar size="xl">
              <!-- todo: заменить аватар и содержание поста            -->
              <img src="https://cdn.quasar.dev/img/avatar2.jpg"/>
            </q-avatar>
          </q-item-section>
          <q-item-section>
            <q-item-label class="text-subtitle1">
              <strong>Author's name</strong>
              <span class="text-grey-7 q-ml-xs"> @author_tag </span>
              <br class="lt-md"/>
              &bull; {{ howLongAgo(post.date) }}
            </q-item-label>
            <q-item-label class="post-text text-body1 q-pt-xs">
              {{ post.text }}
            </q-item-label>
            <div class="row justify-between q-mt-md">
              <q-btn
                @click="toggleLike(post)"
                flat
                round
                :color="post.liked ? 'red-5' : 'grey'"
                size="sm"
                :icon="post.liked ? 'fas fa-heart' : 'far fa-heart'"
              />
              <q-btn flat round color="grey" size="sm" icon="reply"/>
              <q-btn flat round color="grey" size="sm" icon="comment"/>
              <q-btn
                flat
                round
                color="grey"
                size="sm"
                icon="delete"
                @click="deletePost(post)"
              />
            </div>
          </q-item-section>
        </q-item>
      </transition-group>
    </q-list>
  </div>
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

