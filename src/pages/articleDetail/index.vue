<template>
  <div>
    <wxParse :content="article" @preview="preview" @navigate="navigate"/>
  </div>
</template>

<script>
import wxParse from 'mpvue-wxparse'
import marked from 'marked'
import { mapState } from 'vuex'
// const R = require('ramda')
const Fly = require('flyio/dist/npm/wx')
const fly = new Fly()
export default {
  components: {
    wxParse
  },
  data() {
    return {
      item: {},
      articleId: '',
      article: '<div>没有文章</div>',
      articleList: []
    }
  },
  computed: {
    ...mapState(['imageCDN', 'articleUrl'])
  },
  methods: {
    preview(src, e) {
      // do something
      console.log('src')
    },
    navigate(href, e) {
      // do something
      console.log('herf')
    }
  },
  onShow() {},
  async mounted() {
    const articleId = this.$root.$mp.query.articleId
    let articleData = await fly.get(
      `${this.articleUrl}/api/v1/posts/${articleId}`
    )
    articleData = articleData.data.post
    wx.setNavigationBarTitle({
      title: articleData.title
    })
    // console.log(articleData)
    articleData =
      articleData.description + articleData.content + articleData.recommendUrl

    // 支持marked
    this.article = marked(articleData)
  }
}
</script>

<style>
@import url('~mpvue-wxparse/src/wxParse.css');
</style>
