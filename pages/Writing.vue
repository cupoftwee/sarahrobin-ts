<template>
  <DefaultLayout title="Writing" mode="rainbow" glassBorderB>
     <section class="bg-white">
        <post-item 
          v-for="edge in $page.posts.edges" 
          :key="edge.node.id" 
          :post="edge.node" 
        />
      </section>

      <pagination 
        v-if="$page.posts.pageInfo.totalPages > 1"
        :info="$page.posts.pageInfo" 
      />
  </DefaultLayout>
</template>

<script>
const config = {
  "trailingSlash": true,
  "pathPrefix": "",
  "titleTemplate": "%s | Sarah Robin",
  "siteUrl": "https://www.sarahrobin.dev",
  "version": "0.7.23",
  "catchLinks": true
};
import SiteFooter from '@/components/FooterMenu'
import PostItem from '@/components/PostItem'
import Pagination from '@/components/Pagination'
import NavMenu from '@/components/NavMenu'
import Divider from '@/components/Divider'

export default {
  components: {
    PostItem,
    Pagination,
    SiteFooter,
    NavMenu,
    Divider
  },
  metaInfo () {
    return {
      title: this.config.siteName,
      meta: [],
    }
  },
  computed: {
    config () {
      return config
    }
  },
}
</script>

<page-query>
  query AllWriting ($page: Int) {
    posts: allPost (page: $page, perPage: 6, sortBy: "date") @paginate {
      totalCount
      pageInfo {
        totalPages
        currentPage
      }
      edges {
        node {
          id
          title
          datetime: date (format: "YYYY-MM-DD HH:mm:ss")
          content
          excerpt
          description
          path
          tags {
            id
            title
            path
          }
          author {
            id
            title
            path
          }
        }
      }
    }
  }
</page-query>