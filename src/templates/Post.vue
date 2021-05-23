<template>
  <Layout>
    <!-- Page Header-->
    <header
      class="masthead"
      :style="{backgroundImage: `url(${GRIDSOME_API_URl + $page.post.cover.url}`}"
    >
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="post-heading">
              <h1>{{$page.post.title}}</h1>
              <span class="meta">
                Posted by
                <a href="#!">Start Bootstrap</a>
                on {{dayjs($page.post.created_at).format('YYYY-MM-DD HH:mm:ss')}}
              </span>
            </div>
          </div>
        </div>
      </div>
    </header>
    <!-- Post Content-->
    <article>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto" v-html="mdToHtml($page.post.content)">
          </div>
        </div>
      </div>
    </article>
    <hr />
  </Layout>
</template>
<page-query>
  query ($id: ID!) {
    post: strapiPosts (id: $id) {
      id
      title
      content
      cover {
        url
      },
      tags {
        id
        title
      },
      created_at
    }
  }
</page-query>
<script>
import MarkdownIt from 'markdown-it'
import dayjs from 'dayjs'
const md = new MarkdownIt()

export default {
  name: "PostPage",
  methods: {
    dayjs,
    mdToHtml (markdown) {
      return md.render(markdown)
    }
  }
};
</script>

<style></style>
