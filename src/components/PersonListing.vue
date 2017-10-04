<template>
  <div class="vm-listing o-media">
    <div class="o-media__figure" v-if="person.included">
      <a :href="person.path" class="category-brand__thumbnail"><img :src="person.included.attributes.url" :alt="person.relationships.field_sf_primary_image.data.meta.alt" /></a>
    </div>
    <div class="o-media__body">
      <h3 class="vm-listing__title"><a :href="person.path">{{ person.attributes.title }}</a></h3>
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
