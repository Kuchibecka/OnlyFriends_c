<template>
  <q-page>
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
              <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
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
    <q-separator class="post-separator" />

<!--    <KeepAlive>-->
    <PostList v-model="posts"/>
<!--    </KeepAlive>-->

  </q-page>
</template>

<script setup>
import {onMounted, ref} from "vue";
import db from "src/boot/firebase";
import {addDoc, collection, onSnapshot, orderBy, query} from "firebase/firestore";
import PostList from "components/HomePage/PostList.vue";

let newPostText = ref("");
let posts = ref([]);



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

onMounted(() => {
  const q = query(collection(db, "posts"), orderBy("date"));
  // eslint-disable-next-line no-unused-vars
  const unsubscribe = onSnapshot(q, (snapshot) => {
    snapshot.docChanges().forEach((change) => {
      let postChange = change.doc.data();
      postChange.id = change.doc.id;
      if (change.type === "added") {
        // console.log("New post: ", postChange);
        posts.value.unshift(postChange);
      }
      if (change.type === "modified") {
        let index = posts.value.findIndex((post) => post.id === postChange.id);
        // console.log("posts[index]: ", posts.value[index]);
        Object.assign(posts.value[index], postChange);
      }
      if (change.type === "removed") {
        // console.log("Removed post: ", postChange);
        let index = posts.value.findIndex((post) => post.id === postChange.id);
        posts.value.splice(index, 1);
      }
    });
  });
});
</script>

<style lang="sass">
.new-post
  textarea
    font-size: 19px
    line-height: 1.4 !important

.post-separator
  height: 12px
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4
  background: $blue-1

</style>
