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
        <q-icon
          class="q-pa-sm"
          size="lg"
          color="primary"
          name="fa-solid fa-fan"
        />
        OnlyFriends
      </q-toolbar-title>

      <PageSelector />
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

      <q-carousel
        v-model="slide"
        transition-prev="scale"
        transition-next="scale"
        swipeable
        animated
        control-color="grey-8"
        navigation
        padding
        height="600px"
        class="q-ma-md text-black shadow-1 rounded-borders"
      >
        <q-carousel-slide name="style" class="column no-wrap">
          <!-- todo: допилить карусель с рекомендациями          -->
          <div class="col">
            <q-card class="my-card">
              <q-img src="https://cdn.quasar.dev/img/parallax2.jpg">
                <div class="absolute-bottom">
                  <div class="text-h6">Our Changing Planet</div>
                  <div class="text-subtitle2">by John Doe</div>
                </div>
              </q-img>
            </q-card>
          </div>
          <div class="col">
            <q-card class="my-card">
              <q-img src="https://cdn.quasar.dev/img/parallax2.jpg">
                <div class="absolute-bottom">
                  <div class="row">
                    <div class="col-8">
                      <q-avatar size="md-md">
                        <!-- todo: заменить аватар и содержание поста            -->
                        <img src="https://cdn.quasar.dev/img/avatar2.jpg" />
                      </q-avatar>
                    </div>
                    <div class="col-4">
                      <strong>Author's name</strong>
                      <span class="text q-ml-xs"> @author_tag </span>
                    </div>
                  </div>
                </div>
              </q-img>
            </q-card>
          </div>
          <div class="col full-width">
            <q-card class="my-card">
              <q-img src="https://cdn.quasar.dev/img/parallax2.jpg">
                <div class="row">
                  <div class="col-3">
                    <q-avatar size="md-md">
                      <!-- todo: заменить аватар и содержание поста            -->
                      <img src="https://cdn.quasar.dev/img/avatar2.jpg" />
                    </q-avatar>
                  </div>
                  <div class="col-8">
                    <strong>Author's name</strong>
                    <span class="text q-ml-xs"> @author_tag </span>
                  </div>
                </div>
              </q-img>
            </q-card>
          </div>
        </q-carousel-slide>
        <q-carousel-slide name="tv" class="column no-wrap flex-center">
          <q-icon name="search" size="56px" />
          <div class="q-mt-md text-center">{{ lorem }}</div>
        </q-carousel-slide>
        <q-carousel-slide name="layers" class="column no-wrap flex-center">
          <q-icon name="home" size="56px" />
          <div class="q-mt-md text-center">{{ lorem }}</div>
        </q-carousel-slide>
        <q-carousel-slide name="map" class="column no-wrap flex-center">
          <q-icon name="search" size="56px" />
          <div class="q-mt-md text-center">{{ lorem }}</div>
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
import PageSelector from "components/LeftDrawer/PageSelector.vue";

export default {
  components: { PageSelector },
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
