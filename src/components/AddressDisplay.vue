<template>
  <div>
    <template v-if="address.organization">{{ address.organization }},</template>
    {{ address.address_line1 }}
    {{ address.address_line2 }}
    {{ address.locality }},
    {{ address.administrative_area }}
    {{ address.postal_code }}
    <template v-if="address.country_code !== 'US'">{{ address.country_code }}</template>
  </div>
</template>

<script>
  export default {
    props: ['address']
  }
</script>
