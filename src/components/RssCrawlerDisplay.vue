<template>
  <div class="hello">
    <h2>RssCrawler</h2>
    <div class="select is-fullwidth">
      <select v-model="selected">
        <option v-for="option in options" :key="option">
          {{ option }}
        </option>
      </select>
    </div>
    <button class="button  is-primary" v-bind:disabled= "options.length == 0"  @click="getFeeds">Get Url</button>
    <ul>
      <li style="display:block;" v-for="feed in feedList" :key="feed.link">
        *********************************************
        <br/>
        Author: {{feed.author}}
        <br/>
        Title: {{feed.title}}
        <br/>
        Date: {{feed.date}}
        <br/>
        Link: {{feed.link}}
        <br/>
        *********************************************
        <br/>
      </li>
    </ul>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
Vue.use(VueResource)

export default {
  name: 'RssCrawler',
  data () {
    return {
      selected: '',
      options: [],
      feedList: [],
      feed: {
        author: '',
        title: '',
        date: '',
        link: ''
      }
    }
  },
  created: function () {
    this.getRssFeedChannels()
  },

  methods: {
    getRssFeedChannels: function () {
      this.$http.get('http://localhost:8080/rss/getAllChannels')
        .then(response => {
          for (var key in response.body) {
            this.options.push(response.body[key])
          }
          this.selected = response.body[0]
        })
    },
    getFeeds: function () {
      this.$http.get('http://localhost:8080/rss/getRssFeeds?rssName=' + this.selected)
        .then(response => {
          this.feedList = []
          this.feed.author = ''
          this.feed.title = ''
          this.feed.link = ''
          this.feed.date = new Date()
          for (var key in response.body) {
            this.feed.author = response.body[key].author
            this.feed.title = response.body[key].title
            this.feed.date = response.body[key].publishedDate
            this.feed.link = response.body[key].link
            this.feedList.push(this.feed)
          }
        })
      this.feed.author = ''
      this.feed.title = ''
      this.feed.link = ''
      this.feed.date = new Date()
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
