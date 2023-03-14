<template>
  <q-page>
    <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
      <div class="col">
        <q-input
          bottom-slots
          v-model="newPostData"
          placeholder="Compose new post..."
          class="new-post"
          counter maxlength="280"
          :dense="dense"
          autogrow
        >
          <template v-slot:before>
            <q-avatar size="xl">
              <!--todo: заменить на аватар пользователя     -->
              <img src="https://cdn.quasar.dev/img/avatar5.jpg">
            </q-avatar>
          </template>

          <template v-slot:after>

          </template>
        </q-input>
      </div>
      <div class="col-shrink">
        <q-btn
          class="q-mb-lg"
          :disable="!newPostData"
          unelevated
          rounded color="primary"
          label="Post!"
        />
      </div>
    </div>
    <q-separator
      class="post-separator"
    />

    <q-list>
      <q-item class="q-py-md">
        <q-item-section avatar top>
          <q-avatar size="xl">
<!-- todo: заменить аватар и содержание поста            -->
            <img src="https://cdn.quasar.dev/img/avatar2.jpg">
          </q-avatar>
        </q-item-section>
        <q-item-section>
          <q-item-label class="text-subtitle1">
            <strong>Author's name</strong>
            <span class="text-grey-7">
              @author_tag
            </span>
          </q-item-label>
          <q-item-label class="post-text text-body1">
            Post text, post text, post text post text post text jhsdjkf hjsdhjf hsdhkjfj khjskdhfj ksh djkfhk jsdf h.


            PPPPPost text post text post text post text

            - post text
            - post text
            - post text?
            - post text!
            - post text...
          </q-item-label>
          <div class="row justify-between q-mt-md">
            <q-btn
              flat
              round
              color="grey"
              size="sm"
              icon="comment"
            />
            <q-btn
              flat
              round
              color="grey"
              size="sm"
              icon="favorite_border"
            />
          </div>
        </q-item-section>

        <q-item-section side top>
          1 min ago
        </q-item-section>
      </q-item>

    </q-list>

  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageHome",
  data() {
    return {
      newPostData: ""
    };
  }
});
</script>

<style lang="sass">
.new-post
  textarea
    font-size: 19px
    line-height: 1.4 !important
.post-separator
  height: 10px
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4
.post-text
  white-space: pre-line
</style>
