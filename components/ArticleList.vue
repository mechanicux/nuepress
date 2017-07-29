<template>
  <div>
    <article class="articleList" v-for="article in articles" :key="article.id">
      <div class="row date">
        <span v-html="timestamp(article.date)"></span>&nbsp;–&nbsp;<nuxt-link class="topic" v-for="topic in article._embedded['wp:term'][0]" :to="`/topics/${topic.slug}`" :key="topic.id" v-html="topic.name"></nuxt-link>
      </div>
      <nuxt-link :to="`/${article.slug}`" class="row">
        <div class="col">
          <img v-if="article._embedded['wp:featuredmedia']" :src="article._embedded['wp:featuredmedia'][0].media_details.sizes.thumbnail.source_url">
        </div>
        <div class="col">
          <h2 v-html="article.title.rendered"></h2>
          <div v-html="article.excerpt.rendered"></div>
        </div>
      </nuxt-link>
    </article>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  props: ['articles'],

  methods: {
    timestamp (date) { return moment(date).format('MMM d') }
  }
}
</script>

<style lang="scss">
@import './assets/css/vars.scss';

section {
  margin: 0 auto;
}

article.articleList {
  & + article {
    border-top: 1px dotted lighten($primary, 20%);
    margin-top: 32px;
    padding-top: 32px;
  }

  .row {
    display: flex;

    & + .row {
      margin-top: 16px;
    }

    .col {
      display: flex;
      flex-direction: column;
    }
  }

  .date {
    font-family: 'Roboto', sans-serif;
    font-size: 70%;
    text-transform: uppercase;

    .topic + .topic::before {
      content: ', ';
      color: $primary;
    }

    a:hover {
      color: $accent;
    }
  }

  h2 {
    color: #111;
    font-size: 1.1rem;
    font-weight: 400;
    margin-bottom: 8px;
    margin-top: -6px;
  }

  img {
    height: auto;
    margin: 0 22px 0 0;
    max-width: 150px;
  }

  a {
    color: $primary;
    transition: 0.1s;
    text-decoration: none;

    &:hover {
      color: darken($primary, 30%)
    }
  }

  p {
    margin: 0;
  }
}
</style>