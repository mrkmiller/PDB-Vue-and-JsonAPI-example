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
        this.people = result.data;
        // Add the Includes to the data.
        this.applyIncludes(result.included);
        // Add the path since JsonAPI doesn't include one.
        this.createMissingPath();
      });
    },

    applyIncludes (included) {
      // Take the includes from the jsonapi return which contain images and add
      // them to the Person object
      let i;
      for (i = 0; i < this.people.length; i++) {
        if (this.people[i].relationships.field_sf_primary_image.data) {
          let imageId = this.people[i].relationships.field_sf_primary_image.data.id;

          // Now find the relationship included and attach it to the person
          let n;
          for (n = 0; n < included.length; n++) {
            if (included[n].id === imageId) {
              this.people[i].included = included[n];
            }
          }
        }
      }
    },

    createMissingPath () {
      for (let i = 0; i < this.people.length; i++) {
        this.people[i].path = '/node/' + this.people[i].attributes.nid;
      }
    }
  }
}
</script>
