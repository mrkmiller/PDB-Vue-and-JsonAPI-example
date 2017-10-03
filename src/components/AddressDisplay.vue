<template>
  <div>
    <span v-if="address.organization">{{ address.organization }},</span>
    {{ address.address_line1 }}
    {{ address.address_line2 }}
    {{ address.locality }},
    {{ address.administrative_area }}
    {{ address.postal_code }}
    <span v-if="address.country_code !== 'US'">{{ address.country_code }}</span>
  </div>
</template>

<script>
  export default {
    props: ['address']
  }
</script>
