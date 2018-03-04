<template>
  <article class="article">
    <header>
      <h3 class="title">{{post.title}}</h3>
      <a :href="post.url" target="_blank" rel="noopener">[Issue URL]</a>
      <p>{{post.createdAt}}</p>
      <div class="author">
        <img :src="post.author.avatarUrl">
        <p class="author-name">{{post.author.login}}</p>
      </div>
    </header>
    <div class="body">
      <vue-markdown
        class="marked"
        :source="editBody"
        :anchorAttributes="{
          target: '_blank',
          rel: 'noopener'
        }"/>
      <div
        v-if="isDev && isEditing"
        class="edit"
        >
        <textarea
          v-model="editBody"
          ref="editbody"/>
      </div>
    </div>
    <div v-if="isDev">
      <button @click="isEditing = !isEditing">{{isEditing ? '戻る' : '編集'}}</button>
      <div v-if="isEditing && post.body !== editBody" @click="isEditing = false">
        <button @click="initBody">discard</button>
        <button @click="save">save</button>
      </div>
    </div>
  </article>
</template>

<script>
import VueMarkdown from 'vue-markdown'

export default {
  components: {
    VueMarkdown
  },
  props: {
    post: { type: Object }
  },
  data() {
    return {
      editBody: '',
      isEditing: false,
      isDev: process.env.NODE_ENV === 'development'
    }
  },
  created() {
    this.initBody()
  },
  methods: {
    initBody() {
      this.editBody = this.post.body
    },
    async save() {
      try {
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~assets/css/mixin/_mediaquery.scss';
$date-size: 50px;
$date-pad: 6px;

.article {
  margin: 2rem 0;
  padding: 1rem;
  border-radius: 1rem;

  background: #fff;

  .title {
    font-size: 1.8rem;
    margin: 0;
  }
  .author {
    display: grid;
    grid-template-columns: auto auto;
    img {
      width: 40px;
    }
  }
  .body {
    border: 1px solid #eee;
    padding: 1rem;
  }
}

.body {
  position: relative;
  min-height: 3rem;
  .marked {
    font-size: 1.1rem;
    line-height: 1.5;
  }
  .edit {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;

    textarea {
      width: 100%;
      height: 100%;
    }
  }
}
</style>