<template>
  <div class="categoryPage">
    <section>
      <div class="title">
        <!-- <h1>{{ $route.params.category }}</h1> -->
      </div>
      <div v-infinite-scroll="loadMore" class="articles">
      <SingleArticle v-for="(article, index) in apps" :key="index" :data="article"/>
    </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'defaultLayout',
  data(){
    return{
      apps: [],
      appsLoaded: null
    }
  },
  methods: {
    async loadMore(){
        this.appsLoaded = await this.$strapi.$apps.find({
        category: this.$route.params.category,
        _sort: 'title:ASC',
        _start: this.apps.length,
        _limit: 12
      })
      this.apps = this.apps.concat(this.appsLoaded)
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700;900&family=Poppins:wght@100;200;400;600;700;800;900&display=swap');

body{
  font-family: 'Poppins', sans-serif;
}

.categoryPage{
  padding: 0 32px;

  section{
    .title{
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 96%;

      input{
        height: 32px;
      }
    }
  }
  .articles{
    display: flex;
    flex: 1;
    width: 100%;
    flex-wrap: wrap;
    justify-content: center;

    .article{
      @include phone-only{
        width: 40%;
      }
    }
  }
}
</style>
