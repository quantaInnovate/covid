<template>
  <b-row>
    <b-col v-for="(a_hospitals, index) in list_hospitals" :key="index" md="12">
      <p class="col-md">{{ a_hospitals['properties'].NAME }}</p>
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
    }
  }
}
</script>
