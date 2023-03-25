<template>
  <q-page>
    <NewPostForm />
    <q-separator class="post-separator" />

    <!--    <KeepAlive>-->
    <PostList v-model="posts" />
    <!--    </KeepAlive>-->
  </q-page>
</template>

<script setup>
import { onMounted, ref } from "vue";
import db from "src/boot/firebase";
import { collection, onSnapshot, orderBy, query } from "firebase/firestore";
import PostList from "components/HomePage/PostList.vue";
import NewPostForm from "components/HomePage/NewPostForm.vue";

let posts = ref([]);

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
.post-separator
  height: 12px
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4
  background: $blue-1
</style>
