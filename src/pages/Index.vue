<template>
  <Layout>
    <!-- Page Header-->
    <header class="masthead" :style="{backgroundImage: `url(${GRIDSOME_API_URl + general.cover.url}`}">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>{{general.title}}</h1>
              <span class="subheading">{{general.subtitle}}</span>
            </div>
          </div>
        </div>
      </div>
    </header>
    <!-- Main Content-->
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="post-preview" v-for="post in postList" :key="post.node.id">
            <g-link :to="`/post/${post.node.id}`">
              <h2 class="post-title">
                {{post.node.title}}
              </h2>
            </g-link>
            <p class="post-meta">
              Posted by
              <g-link to="/">Zlx Blog</g-link>
              on {{dayjs(post.node.created_at).format('YYYY-MM-DD HH:mm:ss')}}
            </p>
            <p>
              <g-link :to="'/tag/'+tag.id" v-for="tag in post.node.tags" :key="tag.id">{{tag.title}}</g-link>
            </p>
            <hr />
          </div>
          <!-- Pager-->
        <Pager :info="pageInfo"/>
        </div>
      </div>
    </div>
    <hr />
  </Layout>
</template>
<page-query>
query ($page: Int) {
  posts: allStrapiPosts (perPage: 2, page: $page) @paginate{
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
        tags {
          id
          title
        }
        created_at
      }
    }
  },
  allStrapiGeneral {
    edges {
      node {
        id
        title
        subtitle
        cover {
          url
        }
      }
    }
  }
}
</page-query>
<script>
import { Pager } from 'gridsome'
import dayjs from 'dayjs'
export default {
  name: "HomePage",
  methods: {
    dayjs
  },
  components: {
    Pager
  },
  computed: {
            // created_by {
        //   id
        //   firstname
        //   lastname
        // }
    postList () {
      return this.$page.posts.edges
    },
    pageInfo () {
      return this.$page.posts.pageInfo
    },
    general () {
      return this.$page.allStrapiGeneral.edges[0].node
    }
  }
};
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>
