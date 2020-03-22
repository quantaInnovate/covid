<template>
  <b-container class="bv-example-row">
    <b-row>
      <b-col id="googleMap" ref="googleMap"></b-col>
    </b-row>
  </b-container>
</template>

<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCjD4swxA8fo0vOcuGItqwlxF2ol2O2cgM"></script>
<!-- <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCjD4swxA8fo0vOcuGItqwlxF2ol2O2cgM"></script> -->
<script>
// import Logo from '~/components/Logo.vue'
import axios from 'axios'
export default {
  // components: {
  //   Logo
  // },
  data: () => ({
    map: null,
    items: []
  }),
  mounted: function() {
    this.submitMessage()
    this.datajs()
    this.map = new window.google.maps.Map(this.$refs['googleMap'], {
      center: { lat: -25.344, lng: 131.036 },
      zoom: 4
    })
    new window.google.maps.Marker({
      position: { lat: -25.344, lng: 131.036 },
      map: this.map
    })
  },
  methods: {
    submitMessage() {
      console.log('siomtine') // something
    },
    datajs() {
      axios.get('/hospital-lab-covid19.json').then((response) => {
        console.log(response.data)
        //todo:: get data complete
      })
    }
  }
}
</script>

<style lang="scss">
#googleMap {
  height: 500px;
  background: gray;
}
</style>
