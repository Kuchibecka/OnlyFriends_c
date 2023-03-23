<template>
  <div>
    <q-list separator>
      <transition-group
        appear
        enter-active-class="animated fadeInDown"
        leave-active-class="animated fadeOutDown"
      >
        <q-item class="q-py-md" v-for="(post, index) in modelValue" :key="post.id">
<!--   eslint-disable-next-line vue/no-mutating-props       -->
          <SinglePost v-model="modelValue[index]" />
        </q-item>
      </transition-group>
    </q-list>
  </div>
</template>

<script setup>

import SinglePost from "components/HomePage/SinglePost.vue";

defineProps({
  modelValue: Array,
});

</script>

