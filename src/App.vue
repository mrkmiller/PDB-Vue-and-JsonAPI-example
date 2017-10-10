<template>
  <div id="app-directory">
    <div class="u-space-bottom">
      <label for="displaypicker">Select how to display people:</label>
      <select v-model="currentView" id="displaypicker">
        <option value="person-teaser">Teaser</option>
        <option value="person-listing">Listing</option>
        <option value="person-card">Card</option>
      </select>
    </div>
    <component :is="currentView" v-for="person in people" :person="person" :key="person.id"></component>
  </div>
</template>

<script>
import PersonTeaser from './components/PersonTeaser.vue'
import PersonListing from './components/PersonListing.vue'
import PersonCard from './components/PersonCard.vue'
import jsonapiParse from 'jsonapi-parse';

export default {
  name: 'app-directory',
  components: { PersonTeaser, PersonListing, PersonCard },
  data () {
    return {
      people: [],
      currentView: 'person-teaser'
    }
  },
  created () {
    this.getPeople();
  },
  methods: {
    getPeople () {
      const url = 'http://sitefarm.acquia/jsonapi/node/sf_person?sort=field_sf_last_name&include=field_sf_primary_image';
      // Fetch the data via ajax
      fetch(url)
        .then(response => response.json())
        .then(result => {
          this.people = jsonapiParse.parse(result).data;
      });
    }
  }
}
</script>
