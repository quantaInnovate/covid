<template>
  <b-row>
    <b-col md="12" class="row d-flex justify-content-between">
      <h4 class="col-md-6">รายชื่อโรงพยาบาล</h4>
      <div class="col-md-4">
        <!-- <select id name class="col-md custom-select">
          <option value="not">จังหวัด</option>
        </select> -->
      </div>
      <div
        v-for="(a_noti, index) in list_hospitals"
        :key="index"
        class="col-md-4"
      >
        <b-col class="mt-3 border p-2">
          <p class="m-0">{{ a_noti.properties.NAME }}</p>
          <p class="m-0">รูปแบบ: {{ a_noti.properties.TYPE }}</p>
        </b-col>
        <b-row class="m-0">
          <div
            class="col-6 text-center bg-success p-1"
            @click="direction(a_noti.properties.Lat, a_noti.properties.Long)"
          >
            <i class="fas fa-phone text-white"></i>
          </div>
          <div
            class="col-6 text-center p-1 bg-secondary"
            @click="tel(a_noti.properties.NAME)"
          >
            <i class="fas fa-map-marker-alt text-white"></i>
          </div>
        </b-row>
      </div>
    </b-col>
  </b-row>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    list_hospitals: null
  }),
  mounted() {
    this.getdata_json()
  },
  methods: {
    getdata_json() {
      axios
        .get('/hospital-lab-covid19.json')
        .then((response, error) => {
          // console.log(response.data)
          this.list_hospitals = response.data
          console.log(response.data)
        })
        .catch(function(error) {
          // handle error
          console.log(error)
        })
    },
    direction(lat, lng) {
      window.open(
        'https://maps.google.com/?saddr=Current+Location&daddr=loc:' +
          lat +
          ',' +
          lng
      )
    },
    tel(number) {
      console.log(number)
      window.open('tel:' + number)
    }
  }
}
</script>
