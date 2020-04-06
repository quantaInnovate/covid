<template>
  <div>
    <navigation />
    <b-container class="bv-example-row">
      <nuxt />
    </b-container>
  </div>
</template>

<script>
import navigation from '~/components/nav.vue'

export default {
  components: {
    navigation
  }
}
</script>
