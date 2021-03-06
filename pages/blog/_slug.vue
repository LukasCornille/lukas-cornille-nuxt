<template>
  <main>
    <section>
      <Post :post="post" />
    </section>
  </main>
</template>

<script lang="ts">
import Vue from 'vue';
import prismicDom from 'prismic-dom';
import Post from '../../components/Blog/Post.vue';
import { GET_POST } from '../../graphql/post';
import { Posts } from '../../graphql/types';
import { prismic } from '~/graphql/prismic';

export default Vue.extend({
  components: { Post },
  async asyncData({ params, payload }) {
    if (payload) return { post: payload };

    const { data }: Posts = await prismic.query({
      query: GET_POST,
      variables: { slug: params.slug },
    });

    return { post: data.allPosts.edges[0] };
  },
  head() {
    return {
      // @ts-ignore
      title: this.post.node.title[0].text,
      meta: [
        {
          hid: 'description',
          name: 'description',
          // @ts-ignore
          content: prismicDom.RichText.asText(this.post.node.summary),
        },
      ],
      link: [
        {
          rel: 'stylesheet',
          href: 'https://fonts.googleapis.com/css2?family=Fira+Code:wght@500&display=swap',
        },
      ],
    };
  },
});
</script>

<style>
section {
  padding-top: calc(80px + 1rem);
}

.block-img img {
  margin: auto;
}

.hljs {
  display: block;
  overflow-x: auto;
  padding: 0.5em;
  color: #abb2bf;
  background: #282c34;
}

.hljs-comment,
.hljs-quote {
  color: #5c6370;
  font-style: italic;
}

.hljs-doctag,
.hljs-keyword,
.hljs-formula {
  color: #c678dd;
}

.hljs-section,
.hljs-name,
.hljs-selector-tag,
.hljs-deletion,
.hljs-subst {
  color: #e06c75;
}

.hljs-literal {
  color: #56b6c2;
}

.hljs-string,
.hljs-regexp,
.hljs-addition,
.hljs-attribute,
.hljs-meta-string {
  color: #98c379;
}

.hljs-built_in,
.hljs-class .hljs-title {
  color: #e6c07b;
}

.hljs-attr,
.hljs-variable,
.hljs-template-variable,
.hljs-type,
.hljs-selector-class,
.hljs-selector-attr,
.hljs-selector-pseudo,
.hljs-number {
  color: #d19a66;
}

.hljs-symbol,
.hljs-bullet,
.hljs-link,
.hljs-meta,
.hljs-selector-id,
.hljs-title {
  color: #61aeee;
}

.hljs-emphasis {
  font-style: italic;
}

.hljs-strong {
  font-weight: bold;
}

.hljs-link {
  text-decoration: underline;
}
</style>
