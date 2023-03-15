<template>
  <q-layout view="lHr Lpr fff">
    <q-header bordered class="bg-white text-black">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" />
        <q-toolbar-title class="text-weight-bold">
          <span class="gt-sm">{{ $route.name }}</span>

          <!--todo: change to OF logo-->
          <q-icon
            class="q-pa-sm lt-md absolute-center"
            size="md"
            color="primary"
            name="fa-solid fa-fan"
          />
        </q-toolbar-title>

        <!--        <q-btn dense flat round icon="menu" @click="toggleRightDrawer" />-->
      </q-toolbar>
    </q-header>

    <!-- left drawer content -->
    <q-drawer
      show-if-above
      v-model="leftDrawerOpen"
      :width="270"
      side="left"
      bordered
    >
      <q-toolbar-title class="text-weight-bold">
        <!--todo: change to OF logo-->
        <q-icon
          class="q-pa-sm"
          size="lg"
          color="primary"
          name="fa-solid fa-fan"
        />
        OnlyFriends
      </q-toolbar-title>

      <div class="q-pa-md" style="max-width: 350px">
        <q-list>
          <q-item to="/" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="home" size="md" />
            </q-item-section>

            <q-item-section class="text-h6 text-weight-bold"
              >Home
            </q-item-section>
          </q-item>

          <q-item to="/notifications" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="notifications" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">Notifications</q-item-section>
          </q-item>

          <q-item to="/messages" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="message" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">Messages</q-item-section>
          </q-item>

          <q-item to="/bookmarks" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="bookmark" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">Bookmarks</q-item-section>
          </q-item>

          <q-item to="/lists" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="list" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">Lists</q-item-section>
          </q-item>

          <q-item to="/subscriptions" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="loyalty" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">Subscriptions</q-item-section>
          </q-item>

          <q-item to="/credit_card" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="credit_card" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">Add card</q-item-section>
          </q-item>

          <q-item to="/profile" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="account_circle" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">My profile</q-item-section>
          </q-item>

          <q-item to="/more" clickable v-ripple exact>
            <q-item-section avatar>
              <q-icon name="more_horiz" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">More</q-item-section>
          </q-item>

          <div class="q-pt-xl" />

          <q-item
            to="/new_post"
            clickable
            v-ripple
            class="bg-primary text-white"
            style="border-radius: 30px"
          >
            <q-item-section avatar>
              <q-icon color="white" name="add" size="md" />
            </q-item-section>

            <q-item-section class="text-h6">NEW POST</q-item-section>
          </q-item>
        </q-list>
      </div>
    </q-drawer>

    <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <!-- right drawer content -->
      <q-input
        class="q-ma-md"
        label="Search OnlyFriends"
        outlined
        rounded
        dense
      >
        <template v-slot:prepend>
          <q-icon name="search" />
        </template>
      </q-input>

      <!-- todo: исправить карусель, добавить карточки с рекомендациями внутрь  -->
      <q-carousel
        v-model="slide"
        transition-prev="scale"
        transition-next="scale"
        swipeable
        animated
        control-color="grey-8"
        navigation
        padding
        arrows
        height="500px"
        class="q-ma-md text-black shadow-1 rounded-borders"
      >
        <q-carousel-slide name="style" class="column no-wrap flex-center">
          <q-icon name="home" size="56px" />
          <div class="q-mt-md text-center text-black">{{lorem}}</div>
        </q-carousel-slide>
        <q-carousel-slide name="tv" class="column no-wrap flex-center">
          <q-icon name="search" size="56px" />
          <div class="q-mt-md text-center">{{lorem}}</div>
        </q-carousel-slide>
        <q-carousel-slide name="layers" class="column no-wrap flex-center">
          <q-icon name="home" size="56px" />
          <div class="q-mt-md text-center">{{lorem}}</div>
        </q-carousel-slide>
        <q-carousel-slide name="map" class="column no-wrap flex-center">
          <q-icon name="search" size="56px" />
          <div class="q-mt-md text-center">{{lorem}}</div>
        </q-carousel-slide>
      </q-carousel>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer elevated class="bg-grey-8 text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
          </q-avatar>
          <div>Title</div>
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const leftDrawerOpen = ref(false);
    const rightDrawerOpen = ref(false);

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },

      rightDrawerOpen,
      toggleRightDrawer() {
        rightDrawerOpen.value = !rightDrawerOpen.value;
      },
      slide: ref("style"),
      lorem:
        "Lorem ipsum dolor, sit amet consectetur adipisicing elit. Itaque voluptatem totam, architecto cupiditate officia rerum, error dignissimos praesentium libero ab nemo.",
    };
  },
};
</script>
