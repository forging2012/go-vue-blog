<template>
  <section class="archive">
    <p class="archive-title">
      <svg class="icon" aria-hidden="true">
        <use xlink:href="#icon-archive"></use>
      </svg>归档
    </p>
    <div class="archive-com-sec archive-cat">
      <p class="sort-title">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-cat"></use>
        </svg>类别
      </p>
      <div class="sort-items">
        <nuxt-link :to="`/search/category/${cat.id}`" v-for="cat in categories" :key="cat.id" v-if="cat.count > 0">{{ cat.name }} ({{ cat.count }})</nuxt-link>
      </div>
    </div>
    <div class="archive-com-sec archive-tag">
      <p class="sort-title">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-tag"></use>
        </svg>标签
      </p>
      <div class="sort-items">
        <nuxt-link :to="`/search/tag/${tag.id}`" v-for="tag in tags" :key="tag.id" v-if="tag.count > 0">{{ tag.name }} ({{ tag.count }})</nuxt-link>
      </div>
    </div>
    <div class="archive-com-sec archive-time">
      <p class="sort-title">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-time"></use>
        </svg>时间
      </p>
      <div class="sort-items">
        <time-line v-for="(intro,index) in introductions[year]" :key="index">
          <router-link :to="'/articles/' + intro.id">{{intro.title}}</router-link>
          <span class="time"> {{ intro.cerate_time }}</span>
        </time-line>
      </div>
    </div>
  </section>
</template>

<script>
  import axios from 'axios';
  import TimeLine from '~/components/TimeLine';
  import moment from 'moment'
  moment.locale('zh-CN')
  export default {
    head () {
      return {
        title: '归档 - Powered by Louyan'
      };
    },
    data(){
      return {
        introductions: {},
        years: [],
        tags:[]
      }
    },
    asyncData () {
      return axios.get(`/article/getAll`).then(res => {
        if (res.data.success === 1) {
          for (let intro of res.data.posts){
            const introYear = moment(intro.create_time).year()
            if (this.years.indexOf(introYear) === -1) {
              this.years.push(introYear)
              this.introductions[introYear] = []
            }
            intro.PublishTime = moment(intro.create_time).format('MM-DD')
            this.introductions[introYear].push(intro)
          }
        }
      });
    },
    components: {
      TimeLine
    },
  }
</script>

<style lang="scss" scoped>
  @import '../assets/sass/app.scss';
  .archive {
    padding-top: 2em;
    background-color: #fff;

  .archive-title {
    margin-bottom: 1.5em;
    font-size: 2.5em;
    color: #333;
    line-height: 1;
    text-align: center;

  .icon {
    width: 0.9em;
    height: 0.9em;
    vertical-align: -0.1em;
    margin-right: 0.3em;
  }
  }
  .archive-com-sec {
    margin-bottom: 1em;

  &:last-child {
     margin-bottom: 0;
   }
  }
  .sort-title {
    font-size: 1.5em;
    padding: 0.5em 0 0.5em 1em;
    box-sizing: border-box;
    border-left: 0.15em solid $base-color;
  .icon {
    width: 0.9em;
    height: 0.9em;
    vertical-align: -0.1em;
    margin-right: 0.2em;
  }
  }
  .sort-items {
    padding: 0.5em 1em;
    font-size: 1.2em;
  a {
    display: inline-block;
    padding: 0.6em 0.8em;
    margin: 0.5em;
    border-radius: 0.2em;
    background-color: rgba(27,31,35,.05);
    color: $base-color;
  }
  }
  }
</style>


