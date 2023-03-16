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
        <q-item class="q-py-md" v-for="post in posts" :key="post.date">
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
</template>

<script>
import { formatDistance } from "date-fns";
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageHome",
  data() {
    return {
      newPostData: "",
      posts: [
        {
          text: "Математи́ческий ана́лиз (классический математический анализ) —",
          date: 1678900010000,
        },
        {
          text: "совокупность разделов математики, соответствующих историческому",
          date: 1678900000600,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678908000000,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900043000,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900005500,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900343000,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900002250,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900055500,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900333000,
        },
        {
          text:
            "Математи́ческий ана́лиз (классический математический анализ) —" +
            " совокупность разделов математики, соответствующих историческому " +
            "разделу под наименованием «анализ бесконечно малых», " +
            "объединяет дифференциальное и интегральное исчисления",
          date: 1678900023400,
        },
        {
          text:
            "На классическом математическом анализе основывается " +
            "современный анализ, который рассматривается как одно из трёх" +
            " основных направлений математики (наряду с алгеброй и геометрией)." +
            " При этом термин «математический анализ» в классическом понимании" +
            " используется, в основном, в учебных программах и материалах[1]." +
            " В англо-американской традиции классическому математическому" +
            " анализу соответствуют программы курсов с наименованием" +
            " «исчисление» (англ. Calculus). ",
          date: 1678903146115,
        },
        {
          text:
            "Предшественниками математического анализа были античный метод" +
            " исчерпывания и метод неделимых. Все три направления, включая" +
            " анализ, роднит общая исходная идея: разложение на бесконечно " +
            "малые элементы, природа которых, впрочем, представлялась авторам " +
            "идеи довольно туманной. Алгебраический подход (исчисление " +
            "бесконечно малых) начинает появляться у Валлиса, Джеймса Грегори" +
            " и Барроу. В полной мере новое исчисление как систему создал" +
            " Ньютон, который, однако, долгое время не публиковал свои " +
            "открытия[2]. ",
          date: 1678993144115,
        },
        {
          text:
            "Официальной датой рождения дифференциального исчисления можно " +
            "считать май 1684 года, когда Лейбниц опубликовал первую статью " +
            "«Новый метод максимумов и минимумов…»[3]. Эта статья в сжатой и" +
            " малодоступной форме излагала принципы нового метода, названного" +
            " дифференциальным исчислением. ",
          date: 420555,
        },
      ],
    };
  },
  methods: {
    howLongAgo(date) {
      return formatDistance(date, new Date(), { addSuffix: true });
    },
    addNewPost() {
      let newPost = {
        text: this.newPostData,
        date: Date.now(),
      };
      this.posts.unshift(newPost);
      this.newPostData = "";
    },
    deletePost(post) {
      // console.log('Delete post: ', post)
      // todo: исправить на идентификатор поста
      let idToDelete = post.date;
      let index = this.posts.findIndex((post) => post.date === idToDelete);
      console.log(index);
      this.posts.splice(index, 1);
    },
  },
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

.post-text
  white-space: pre-line
</style>
