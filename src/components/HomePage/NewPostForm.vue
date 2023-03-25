<template>
  <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
    <div class="col">
      <q-input
          bottom-slots
          v-model="newPostText"
          placeholder="Compose new post..."
          class="new-post"
          counter
          maxlength="280"
          autogrow
      >
        <!--    todo: whatis :dense="dense" param    -->
        <template v-slot:before>
          <q-avatar size="xl">
            <img src="https://cdn.quasar.dev/img/avatar5.jpg"/>
          </q-avatar>
        </template>
      </q-input>
    </div>
    <div class="col-shrink">
      <q-btn
          @click="addNewPost()"
          class="q-mb-lg"
          :disable="!newPostText"
          unelevated
          rounded
          color="primary"
          label="Post!"
      />
    </div>
  </div>
</template>
<script setup>
import {ref} from "vue";
import {addDoc, collection} from "firebase/firestore";
import db from "boot/firebase";

let newPostText = ref("");

async function addNewPost() {
  // let newPost DocumentData = new DocumentData();
  let newPost = {
    text: newPostText.value,
    date: Date.now(),
    liked: false
  };
  const docRef = await addDoc(collection(db, "posts"), newPost);
  console.log(docRef.id);

  newPostText.value = "";
}

</script>
<style lang="sass">
.new-post
  textarea
    font-size: 19px
    line-height: 1.4 !important
</style>
