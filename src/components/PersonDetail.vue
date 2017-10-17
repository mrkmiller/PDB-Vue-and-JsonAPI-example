<template>
  <section>
    <div class="u-align--right u-width--one-third">
      <img v-if="person.field_sf_primary_image" :src="person.field_sf_primary_image.url" :alt="person.field_sf_primary_image.meta.alt" />
    </div>
    <h1>{{ person.title }}</h1>
    <h2>{{ person.field_sf_position_title }}</h2>
    <h3 class="u-space-bottom--small" v-for="unit in person.field_sf_unit">{{ unit }}</h3>
    <ul class="list--pipe u-space-bottom">
      <li v-if="person.field_sf_phone_numbers" v-for="phone in person.field_sf_phone_numbers">
        <a class="icon icon--phone" >{{ phone }}</a>
      </li>
      <li v-if="person.field_sf_emails" v-for="email in person.field_sf_emails">
        <a class="icon icon--link icon--envelope" :href="'mailto:'+email">{{ email }}</a>
      </li>
      <li v-if="person.field_sf_websites" v-for="website in person.field_sf_websites">
        <a class="icon icon--link icon--web" :href="website.uri">{{ website.uri }}</a>
      </li>
      <li class="icon icon--location" v-if="person.field_sf_office_location">{{ person.field_sf_office_location }}</li>
      <li><address-display class="icon icon--map" v-if="person.field_sf_address" :address="person.field_sf_address"></address-display></li>
    </ul>
    <template v-if="person.field_sf_office_hours">
      <strong>Office Hours</strong>
      <p >{{ person.field_sf_office_hours }}</p>
    </template>
    <div v-if="person.body" v-html="person.body.value"></div>

    <template v-if="files.length">
      <h2 class="heading--auxiliary">Documents</h2>
      <ul class="list--download">
        <li v-for="file in files">
          <a class="icon icon--link icon--download" :href="file.url">{{ file.filename }}</a>
        </li>
      </ul>
    </template>

    <template v-if="tags.length">
      <h2 class="heading--auxiliary">Tags</h2>
      <a :href="tag.path.alias" class="tags__tag" v-for="tag in tags">{{ tag.name }}</a>
    </template>

    <template v-if="articles.length">
      <h2 class="heading--auxiliary">Related Articles</h2>
      <article-listing v-for="article in articles" :article="article" :key="article.id"></article-listing>
    </template>

  </section>
</template>

<script>
  import AddressDisplay from './AddressDisplay.vue'
  import ArticleListing from './ArticleListing.vue'
  import jsonapiParse from 'jsonapi-parse';

  export default {
    components: { AddressDisplay, ArticleListing },
    data () {
      return {
        files: [],
        tags: [],
        articles: []
      }
    },
    props: ['person'],
    created () {
      this.getReferencedContent();
      this.getRelatedArticles()
    },
    methods: {
      getReferencedContent () {
        const url = 'http://sitefarm.acquia/jsonapi/node/sf_person/' + this.person.id + '?include=field_sf_files,field_sf_tags&fields%5Bnode--sf_person%5D=title,field_sf_files,field_sf_tags'

        // Fetch the data via ajax
        fetch(url)
          .then(response => response.json())
          .then(result => {
            let data = jsonapiParse.parse(result).data;
            this.files = data.field_sf_files;
            this.tags = data.field_sf_tags;
          });
      },

      getRelatedArticles () {
        const url = 'http://sitefarm.acquia/jsonapi/node/sf_article?include=field_sf_person_reference,field_sf_primary_image&filter%5Bfield_sf_person_reference.uuid%5D%5Bvalue%5D=' + this.person.id;

        // Fetch the data via ajax
        fetch(url)
          .then(response => response.json())
          .then(result => {
            this.articles = jsonapiParse.parse(result).data;
          });
      }
    }
  }
</script>
