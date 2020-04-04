<template>
  <b-container class="bv-example-row">
    <b-row>
      <b-col md="12">
        <b-row v-if="object_active_now">
          <b-col md="3">
            <div
              class="d-flex justify-content-center align-items-center p-3 shadow-sm border"
            >
              <p class="fs-18 align-self-center m-0">Confirmed:</p>
              <p class="fs-18 align-self-center m-0">
                {{ object_active_now.Confirmed }}
              </p>
            </div>
          </b-col>
          <b-col md="3">
            <div
              class="d-flex justify-content-center align-items-center p-3 shadow-sm border"
            >
              <p class="fs-18 align-self-center m-0">Hospitalized:</p>
              <p class="fs-18 align-self-center m-0">
                {{ object_active_now.Hospitalized }}
              </p>
            </div>
          </b-col>
          <b-col md="3">
            <div
              class="d-flex justify-content-center align-items-center p-3 shadow-sm border"
            >
              <p class="fs-18 align-self-center m-0">Recovered:</p>
              <p class="fs-18 align-self-center m-0">
                {{ object_active_now.Recovered }}
              </p>
            </div>
          </b-col>
          <b-col md="3">
            <div
              class="d-flex justify-content-center align-items-center p-3 shadow-sm border"
            >
              <p class="fs-18 align-self-center m-0">Deaths:</p>
              <p class="fs-18 align-self-center m-0">
                {{ object_active_now.Deaths }}
              </p>
            </div>
          </b-col>
        </b-row>
      </b-col>
      <b-col md="12" class="text-center my-4">
        <h5>Amount Province: {{ amt_province }}</h5>
      </b-col>

      <b-col
        v-for="(a_patiten, index) in patitent_list.Province"
        :key="index"
        md="3"
      >
        <div class="card my-2">
          <div class="card-body">
            <h4 class="card-title text-center">{{ a_patiten }}</h4>
            <p class="card-text text-center">{{ index }}</p>
          </div>
        </div>
      </b-col>
      <b-col v-for="(item, index) in items" :key="index" md="12">
        <p class="col-md">{{ item['properties'].NAME }}</p>
      </b-col>
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
    object_active_now: null,
    patitent_list: [],
    amt_province: null
  }),
  mounted: function() {
    this.getdata_json()
    this.getdata_amount_case()
    this.getdata_sum_paitaint()
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
    },
    getdata_sum_paitaint() {
      axios
        .get('https://covid19.th-stat.com/api/open/cases/sum')
        .then((response, error) => {
          console.log(typeof response.data)
          this.patitent_list = response.data
          this.amt_province = Object.values(response.data.Province).length
          console.log(Object.values(response.data.Province).length)
        })
        .catch(function(error) {
          // handle error
          console.log(error)
        })
    }
  }
}
</script>
