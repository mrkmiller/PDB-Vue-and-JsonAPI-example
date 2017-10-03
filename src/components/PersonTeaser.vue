<template>
  <article class="o-media vm-teaser" :class="{'vm-teaser--featured': person.attributes.field_sf_featured_status}">
    <div class="o-media__figure vm-teaser__figure category category-brand__thumbnail" v-if="person.included">
      <a :href="personLink"><img :src="person.included.attributes.url" :alt="person.relationships.field_sf_primary_image.data.meta.alt" /></a>
    </div>
    <div class="o-media__body">
      <h3 class="vm-teaser__title"><a :href="personLink">{{ person.attributes.title }}</a></h3>
      <ul class="vm-teaser__position" v-if="person.attributes.field_sf_position_title || person.attributes.field_sf_unit">
        <li><b>{{ person.attributes.field_sf_position_title }}</b></li>
        <li v-for="unit in person.attributes.field_sf_unit">{{ unit }}</li>
      </ul>
      <div class="vm-teaser__summary" v-if="person.attributes.body.summary" v-html="person.attributes.body.summary"></div>
      <ul class="vm-teaser__contact list--pipe text--smaller">
        <li v-if="person.attributes.field_sf_phone_numbers" v-for="phone in person.attributes.field_sf_phone_numbers">
          <a class="icon icon--link icon--phone" >{{ phone }}</a>
        </li>
        <li v-if="person.attributes.field_sf_emails" v-for="email in person.attributes.field_sf_emails">
          <a class="icon icon--link icon--envelope" :href="'mailto:'+email">{{ email }}</a>
        </li>
        <li v-if="person.attributes.field_sf_websites" v-for="website in person.attributes.field_sf_websites">
          <a class="icon icon--link icon--web" :href="website.uri">{{ website.uri }}</a>
        </li>
        <li>
          <a class="icon icon--link icon--view-more" :href="personLink">View Listing</a>
        </li>
      </ul>
      <div class="icon icon--location" v-if="person.attributes.field_sf_office_location">{{ person.attributes.field_sf_office_location }}</div>
      <div class="icon icon--map u-space-bottom--small" v-if="person.attributes.field_sf_address"><address-display :address="person.attributes.field_sf_address"></address-display></div>
      <div class="text--smaller" v-if="person.attributes.field_sf_office_hours"><strong>Office Hours</strong> {{ person.attributes.field_sf_office_hours }}</div>
    </div>
  </article>
</template>

<script>
  import AddressDisplay from './AddressDisplay.vue'

  export default {
    components: { AddressDisplay },
    props: ['person'],
    computed: {
      personLink () {
        return '/node/' + this.person.attributes.nid;
      }
    }
  }
</script>
