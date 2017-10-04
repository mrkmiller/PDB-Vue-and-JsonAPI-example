<template>
  <section class="vm-card">
    <header class="vm-card__header o-media">
      <div class="vm-card__figure o-media__figure">
        <a :href="person.path"><img :src="person.included.attributes.url" :alt="person.relationships.field_sf_primary_image.data.meta.alt" /></a>
      </div>
      <div class="o-media__body">
        <h3 class="vm-card__title"><a :href="person.path">{{ person.attributes.title }}</a></h3>
        <div><strong>{{ person.attributes.field_sf_position_title }}</strong></div>
        <div v-for="unit in person.attributes.field_sf_unit">{{ unit }}</div>
      </div>
    </header>

    <div class="vm-card__summary" v-if="person.attributes.body.summary" v-html="person.attributes.body.summary"></div>

    <footer class="vm-card__btn">
      <a :href="person.path" class="btn btn--sm">View Listing</a>
    </footer>
  </section>
</template>

<script>
  export default {
    props: ['person']
  }
</script>
