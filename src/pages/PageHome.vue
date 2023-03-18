<template>
  <q-page>
    <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
      <div class="col">
        <q-input
          bottom-slots
          v-model="newPostData"
          placeholder="Compose new post..."
          class="new-post"
          counter
          maxlength="280"
          autogrow
        >
          <!--    todo: whatis :dense="dense" param    -->
          <template v-slot:before>
            <q-avatar size="xl">
              <!--todo: заменить на аватар пользователя     -->
              <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
            </q-avatar>
          </template>
        </q-input>
      </div>
      <div class="col-shrink">
        <q-btn
          @click="addNewPost()"
          class="q-mb-lg"
          :disable="!newPostData"
          unelevated
          rounded
          color="primary"
          label="Post!"
        />
      </div>
    </div>
    <q-separator class="post-separator" />

    <q-list separator>
      <transition-group
        appear
        enter-active-class="animated fadeInDown"
        leave-active-class="animated fadeOutDown"
      >
        <q-item class="q-py-md" v-for="post in posts" :key="post.id">
          <q-item-section avatar top>
            <q-avatar size="xl">
              <!-- todo: заменить аватар и содержание поста            -->
              <img src="https://cdn.quasar.dev/img/avatar2.jpg" />
            </q-avatar>
          </q-item-section>
          <q-item-section>
            <q-item-label class="text-subtitle1">
              <strong>Author's name</strong>
              <span class="text-grey-7 q-ml-xs"> @author_tag </span>
              <br class="lt-md" />
              &bull; {{ howLongAgo(post.date) }}
            </q-item-label>
            <q-item-label class="post-text text-body1 q-pt-xs">
              {{ post.text }}
            </q-item-label>
            <div class="row justify-between q-mt-md">
              <q-btn flat round color="grey" size="sm" icon="favorite_border" />
              <q-btn flat round color="grey" size="sm" icon="reply" />
              <q-btn flat round color="grey" size="sm" icon="comment" />
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
  </q-page>

  <q-btn @click="looog">
    console.log
  </q-btn>
</template>

<!--<script>
import { formatDistance } from "date-fns";
import { defineComponent } from "vue";
import db from "src/boot/firebase";
import {
  collection,
  query,
  onSnapshot,
  orderBy,
  addDoc,
  deleteDoc,
  doc
} from "firebase/firestore";

export default defineComponent({
  name: "PageHome",
  data() {
    return {
      newPostData: "",
      posts: [],
    };
  },
  methods: {
    howLongAgo(date) {
      // docs: https://date-fns.org/v2.29.3/docs/Getting-Started
      return formatDistance(Number(date), new Date(), { addSuffix: true });
    },
    async addNewPost() {
      let newPost = {
        text: this.newPostData,
        date: Date.now(),
      };
      const docRef = await addDoc(collection(db, "posts"), newPost);
      console.log(docRef.id);

      // this.posts.unshift(newPost);
      this.newPostData = "";
    },
    async deletePost(post) {
      // console.log('Delete post: ', post)
      // todo: исправить на идентификатор поста
      await deleteDoc(doc(db, "posts", post.id));
    },
  },
  mounted() {
    const q = query(collection(db, "posts"), orderBy("date"));
    const unsubscribe = onSnapshot(q, (snapshot) => {
      snapshot.docChanges().forEach((change) => {
        let postChange = change.doc.data();
        postChange.id = change.doc.id;
        if (change.type === "added") {
          // console.log("New post: ", postChange);
          this.posts.unshift(postChange);
        }
        if (change.type === "modified") {
          console.log("Modified post: ", postChange);
        }
        if (change.type === "removed") {
          // console.log("Removed post: ", postChange);
          let index = this.posts.findIndex(post => post.id === postChange.id);
          this.posts.splice(index, 1);
        }
      });
    });

    // eslint-disable-next-line no-constant-condition
    if (false) {
      unsubscribe();
    }
  },
});
</script>-->

<script setup>
import { formatDistance } from "date-fns";
import { onMounted, ref } from "vue";
import db from "src/boot/firebase";
import {
  collection,
  query,
  onSnapshot,
  orderBy,
  addDoc,
  deleteDoc,
  doc
} from "firebase/firestore";

// eslint-disable-next-line no-unused-vars
const name = "PageHome";
let newPostData = ref('')
let posts = ref([])

function howLongAgo(date) {
  // docs: https://date-fns.org/v2.29.3/docs/Getting-Started
  return formatDistance(Number(date), new Date(), { addSuffix: true });
}

async function addNewPost() {
  let newPost = {
    text: newPostData.value,
    date: Date.now(),
  };
  const docRef = await addDoc(collection(db, "posts"), newPost);
  console.log(docRef.id);

  newPostData.value = "";
}

async function deletePost(post) {
  // console.log('Delete post: ', post)
  // todo: исправить на идентификатор поста
  await deleteDoc(doc(db, "posts", post.id));
}

function looog() {
  console.log("posts: ", posts.value);
  console.log("newPostData: ", newPostData.value);
}

onMounted(() => {
  console.log("posts: ", posts.value);
  console.log("newPostData: ", newPostData.value);
  const q = query(collection(db, "posts"), orderBy("date"));
  // eslint-disable-next-line no-unused-vars
  const unsubscribe = onSnapshot(q, (snapshot) => {
    snapshot.docChanges().forEach((change) => {
      let postChange = change.doc.data();
      postChange.id = change.doc.id;
      if (change.type === "added") {
        console.log("New post: ", postChange);
        posts.value.unshift(postChange);
      }
      if (change.type === "modified") {
        console.log("Modified post: ", postChange);
      }
      if (change.type === "removed") {
        // console.log("Removed post: ", postChange);
        let index = posts.value.findIndex(post => post.id === postChange.id);
        posts.value.splice(index, 1);
      }
    });
  });
})

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

.post-text
  white-space: pre-line
</style>
