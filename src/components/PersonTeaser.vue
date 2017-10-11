<template>
  <article class="o-media vm-teaser" :class="{'vm-teaser--featured': person.field_sf_featured_status}">
    <div class="o-media__figure vm-teaser__figure category category-brand__thumbnail" v-if="person.field_sf_primary_image">
      <a :href="person.path.alias"><img :src="person.field_sf_primary_image.url" :alt="person.title" /></a>
    </div>
    <div class="o-media__body">
      <h3 class="vm-teaser__title"><a :href="person.path.alias">{{ person.title }}</a></h3>
      <ul class="vm-teaser__position" v-if="person.field_sf_position_title || person.field_sf_unit">
        <li><b>{{ person.field_sf_position_title }}</b></li>
        <li v-for="unit in person.field_sf_unit">{{ unit }}</li>
      </ul>
      <div class="vm-teaser__summary" v-if="person.body" v-html="person.body.summary"></div>
      <ul class="vm-teaser__contact list--pipe text--smaller">
        <li v-if="person.field_sf_phone_numbers" v-for="phone in person.field_sf_phone_numbers">
          <a class="icon icon--link icon--phone" >{{ phone }}</a>
        </li>
        <li v-if="person.field_sf_emails" v-for="email in person.field_sf_emails">
          <a class="icon icon--link icon--envelope" :href="'mailto:'+email">{{ email }}</a>
        </li>
        <li v-if="person.field_sf_websites" v-for="website in person.field_sf_websites">
          <a class="icon icon--link icon--web" :href="website.uri">{{ website.uri }}</a>
        </li>
        <li>
          <a class="icon icon--link icon--view-more" :href="person.path.alias" @click.prevent="showDetail()">View Listing</a>
        </li>
      </ul>
      <div class="icon icon--location" v-if="person.field_sf_office_location">{{ person.field_sf_office_location }}</div>
      <address-display class="icon icon--map u-space-bottom--small" v-if="person.field_sf_address" :address="person.field_sf_address"></address-display>
      <div class="text--smaller" v-if="person.field_sf_office_hours"><strong>Office Hours</strong> {{ person.field_sf_office_hours }}</div>
    </div>
  </article>
</template>

<script>
  import AddressDisplay from './AddressDisplay.vue'

  export default {
    components: { AddressDisplay },
    props: ['person'],
    methods: {
      showDetail () {
        this.$emit('clicked-show-detail', this.person.id);
      }
    }
  }
</script>
