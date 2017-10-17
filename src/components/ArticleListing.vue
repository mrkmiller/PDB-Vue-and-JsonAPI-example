<template>
  <div class="vm-listing o-media">
    <div class="o-media__figure" v-if="article.field_sf_primary_image">
      <a :href="article.path.alias" class="category-brand__thumbnail"><img :src="article.field_sf_primary_image.url" :alt="article.field_sf_primary_image.meta.alt" /></a>
    </div>
    <div class="o-media__body">
      <h3 class="vm-listing__title"><a :href="article.path.alias">{{ article.title }}</a></h3>
      <div class="vm-listing__submitted">{{ submittedDate }}</div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['article'],
    computed: {
      submittedDate () {
        let date = new Date(this.article.created * 1000);
        return (date.toLocaleString('en-us', { month: "long" })) + ' ' + date.getDate() + ', ' + date.getFullYear()
      }
    },
  }
</script>

<style scoped>
  .o-media__figure {
    width: 135px;
  }
</style>
