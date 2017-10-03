<template>
  <div id="app-directory">
    <person-teaser v-for="person in people" :person="person" :key="person.id"></person-teaser>
  </div>
</template>

<script>
import PersonTeaser from './components/PersonTeaser.vue'

export default {
  name: 'app-directory',
  components: { PersonTeaser },
  data () {
    return {
      people: []
    }
  },
  created () {
    this.getPeople();
  },
  methods: {
    getPeople () {
      const url = 'http://sitefarm.acquia/jsonapi/node/sf_person?include=field_sf_primary_image';
      // Fetch the data via ajax
      fetch(url)
        .then(response => response.json())
        .then(result => {
        this.people = result.data;
        // Add the Includes to the data
        this.applyIncludes(result.included);
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
              console.log(this.people[i]);
              this.people[i].included = included[n];
            }
          }
        }
      }
    }
  }
}
</script>
