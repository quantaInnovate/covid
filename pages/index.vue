<template>
  <b-container class="bv-example-row">
    <b-row>
      <!-- <b-col id="googleMap" ref="googleMap"></b-col> -->
      <b-col md="12">
        <b-row v-if="object_active_now">
          <numbercard
            :topiChospital="object_active_now.Hospitalized"
          ></numbercard>
          <numbercard :topiChospital="object_active_now.Confirmed"></numbercard>
          <numbercard :topiChospital="object_active_now.Deaths"></numbercard>
          <numbercard :topiChospital="object_active_now.Recovered"></numbercard>
        </b-row>
      </b-col>
      <b-col v-for="(item, index) in items" :key="index" md="12">
        <p class="col-md">{{ item['properties'].NAME }}</p>
      </b-col>

      <!-- <b-col-md-12 v-for="item in items" :key="item">
        <p class="col-md">{{ item['properties'].NAME }}</p>
      </b-col-md-12>-->
      <!-- <li v-for="item in items" :key="item">{{ item }}</li> -->
      <!-- {{ items }} -->
    </b-row>
  </b-container>
</template>

<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCjD4swxA8fo0vOcuGItqwlxF2ol2O2cgM"></script>
<!-- <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCjD4swxA8fo0vOcuGItqwlxF2ol2O2cgM"></script> -->
<script>
// import Logo from '~/components/Logo.vue'
import numbercard from '~/components/number_card.vue'
import axios from 'axios'
export default {
  components: {
    //   Logos
    numbercard
  },
  data: () => ({
    map: null,
    items: null,
    object_active_now: null
  }),
  mounted: function() {
    this.getdata_json()
    this.getdata_amount_case()
    // this.map = new window.google.maps.Map(this.$refs['googleMap'], {
    //   center: { lat: -25.344, lng: 131.036 },
    //   zoom: 4
    // })
    // new window.google.maps.Marker({
    //   position: { lat: -25.344, lng: 131.036 },
    //   map: this.map
    // })
  },
  methods: {
    getdata_json() {
      axios
        .get('/hospital-lab-covid19.json')
        .then((response, error) => {
          // console.log(response.data)
          this.items = response.data
          console.log(response.data)
          //todo:: get data complete
        })
        .catch(function(error) {
          // handle error
          console.log(error)
        })
    },
    getdata_amount_case() {
      axios
        .get('https://covid19.th-stat.com/api/open/today')
        .then((response, error) => {
          console.log(typeof response.data)
          this.object_active_now = response.data
        })
        .catch(function(error) {
          // handle error
          console.log(error)
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
