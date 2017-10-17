<template>
  <div class="vm-listing o-media">
    <div class="o-media__figure" v-if="person.field_sf_primary_image">
      <a :href="person.path.alias" class="category-brand__thumbnail" @click.prevent="showDetail()">
        <img :src="person.field_sf_primary_image.url" :alt="person.field_sf_primary_image.meta.alt" />
      </a>
    </div>
    <div class="o-media__body">
      <h3 class="vm-listing__title">
        <a :href="person.path.alias" @click.prevent="showDetail()">{{ person.title }}</a>
      </h3>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['person'],
    methods: {
      showDetail () {
        this.$emit('clicked-show-detail', this.person.id);
      }
    }
  }
</script>

<style scoped>
  .o-media__figure {
    width: 135px;
  }
</style>
