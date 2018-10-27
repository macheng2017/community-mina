<template>
  <div>
    <wxParse :content="article" @preview="preview" @navigate="navigate"/>
  </div>
</template>

<script>
import wxParse from 'mpvue-wxparse'
const R = require('ramda')

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
  onShow() {
    let article = R.filter(R.propEq('id', this.articleId))(this.articleList)
    // console.log(article[0])
    this.article = R.head(article).comments
    wx.setNavigationBarTitle({
      title: R.head(article).stitle
    })
  },
  async mounted() {
    const articleList = await wx.getStorageSync('articleList')
    // console.log(articleList)
    this.articleList = articleList
    // this.item = JSON.parse(this.$root.$mp.query.item)
    const articleId = this.$root.$mp.query.articleId
    // console.log(articleId)
    this.articleId = articleId
  }
}
</script>

<style>
@import url('~mpvue-wxparse/src/wxParse.css');
</style>
