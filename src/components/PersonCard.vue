<template>
  <section class="vm-card u-space-bottom">
    <header class="vm-card__header o-media">
      <div class="vm-card__figure o-media__figure" v-if="person.field_sf_primary_image">
        <a :href="person.path.alias" @click.prevent="showDetail()"><img :src="person.field_sf_primary_image.url" :alt="person.field_sf_primary_image.meta.alt" /></a>
      </div>
      <div class="o-media__body">
        <h3 class="vm-card__title"><a :href="person.path.alias" @click.prevent="showDetail()">{{ person.title }}</a></h3>
        <div><strong>{{ person.field_sf_position_title }}</strong></div>
        <div v-for="unit in person.field_sf_unit">{{ unit }}</div>
      </div>
    </header>

    <div class="vm-card__summary" v-if="person.body" v-html="person.body.summary"></div>

    <footer class="vm-card__btn">
      <a :href="person.path.alias" class="btn btn--sm" @click.prevent="showDetail()">View Listing</a>
    </footer>
  </section>
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
  @media (min-width: 700px) {
    .vm-card {
      float: left;
      padding-right: 1em;
      width: 50%;
    }
  }
</style>
