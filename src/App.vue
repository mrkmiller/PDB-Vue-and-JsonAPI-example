<template>
  <div id="app-directory">
    <section v-if="!detail">
      <div class="u-space-bottom">
        <label for="displaypicker">Select how to display people:</label>
        <select v-model="currentView" id="displaypicker">
          <option value="person-teaser">Teaser</option>
          <option value="person-listing">Listing</option>
          <option value="person-card">Card</option>
        </select>

        <div class="search-form">
          <label for="search-field" class="u-hidden--visually">Search</label>
          <input type="input" placeholder="Search by Name..." id="search-field" class="search-form__input" v-model="nameFilter" @keyup.enter="filterList()">
          <input type="image" class="search-form__submit" alt="Search" @click.prevent="filterList()">
        </div>

      </div>
      <div v-if="loading">Loading...</div>
      <component :is="currentView" v-for="person in people" :person="person" :key="person.id" @clicked-show-detail="showDetail"></component>
      <div v-if="!people.length" class="alert alert--error">Sorry! There are no people matching <strong>"{{ searchedValue }}"</strong></div>
    </section>
    <section v-if="detail">
      <button @click="backToListings()">Back to Listings</button>
      <person-detail :person="detail"></person-detail>
    </section>
  </div>
</template>

<script>
import PersonTeaser from './components/PersonTeaser.vue'
import PersonListing from './components/PersonListing.vue'
import PersonCard from './components/PersonCard.vue'
import PersonDetail from './components/PersonDetail.vue'
import jsonapiParse from 'jsonapi-parse';

export default {
  name: 'app-directory',
  components: { PersonTeaser, PersonListing, PersonCard, PersonDetail },
  data () {
    return {
      people: [],
      loading: true,
      currentView: 'person-teaser',
      nameFilter: '',
      searchedValue: '',
      detail: ''
    }
  },
  created () {
    this.getPeople();
  },
  methods: {
    getPeople () {
      const url = 'http://sitefarm.acquia/jsonapi/node/sf_person';
      let params = '?sort=field_sf_last_name&include=field_sf_primary_image'

      if (this.nameFilter) {
        params += '&filter[title-filter][condition][path]=title'
        params += '&filter[title-filter][condition][value]=' + this.nameFilter
        params += '&filter[title-filter][condition][operator]=CONTAINS'
      }

      // Fetch the data via ajax
      fetch(url + params)
        .then(response => response.json())
        .then(result => {
          this.people = jsonapiParse.parse(result).data;
          this.loading = false;
      });
    },

    filterList () {
      this.loading = true;
      this.searchedValue = this.nameFilter;
      this.getPeople();
    },

    backToListings () {
      this.detail = '';
    },

    showDetail (id) {
      this.detail = this.people.find((person) => person.id === id);
    }
  }
}
</script>
