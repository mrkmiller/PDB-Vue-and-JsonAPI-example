<template>
  <div class="vm-listing o-media">
    <div class="o-media__figure" v-if="person.field_sf_primary_image">
      <a :href="person.path.alias" class="category-brand__thumbnail"><img :src="person.field_sf_primary_image.url" :alt="person.title" /></a>
    </div>
    <div class="o-media__body">
      <h3 class="vm-listing__title"><a :href="person.path.alias">{{ person.title }}</a></h3>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['person']
  }
</script>

<style scoped>
  .o-media__figure {
    width: 135px;
  }
</style>
