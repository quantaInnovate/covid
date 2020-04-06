<template>
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
    <b-col md="12" class="row d-flex justify-content-between">
      <h4 class="col-md-6">
        จำนวนจังหวัดที่มีคนติดเชื้อ: {{ amt_province }} จังหวัด
      </h4>
      <div class="col-md-4">
        <select
          id=""
          name=""
          class="col-md custom-select"
          @change="filter_amt_petient($event)"
        >
          <option value="not">เรียงลำดับ</option>
          <option value="desc">จำนวนมากไปน้อย</option>
          <option value="asc">จำนวนน้อยไปมาก</option>
        </select>
      </div>
    </b-col>
    <b-col v-for="(a_patiten, index) in patitent_list" :key="index" md="3">
      <div class="card my-2">
        <div class="card-body">
          <h5 class="card-title text-center">{{ a_patiten.province }}</h5>
          <p class="card-text text-center">{{ a_patiten.amt }}</p>
        </div>
      </div>
    </b-col>
    <b-col md="12" class="row d-flex justify-content-between">
      <h4 class="col-md-6">ประกาศแจ้งเตือนพื้นที่</h4>
      <div class="col-md-4">
        <select
          id=""
          name=""
          class="col-md custom-select"
          @change="filter_province($event)"
        >
          <option value="not">จังหวัด</option>
          <option
            v-for="(a_province, index) in province"
            :key="index"
            :value="a_province.province_en"
            >{{ a_province.province_th }}</option
          >
        </select>
      </div>
      <div
        v-for="(a_noti, index) in notification_news"
        :key="index"
        class="col-md-6"
      >
        <b-col class="my-2 border p-2">
          <p class="m-0">สถานที่: {{ a_noti.Location }}</p>
          <p class="m-0">ประกาศโดย: {{ a_noti.AnnounceBy }}</p>
          <p class="m-0">เวลา: {{ a_noti.Time }}</p>
          <p class="m-0">จังหวัด: {{ a_noti.Province }}</p>
        </b-col>
      </div>
    </b-col>
  </b-row>
</template>
<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCjD4swxA8fo0vOcuGItqwlxF2ol2O2cgM"></script>
<!-- <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCjD4swxA8fo0vOcuGItqwlxF2ol2O2cgM"></script> -->
<script>
// import Logo from '~/components/Logo.vue'
import numbercard from '~/components/number_card.vue'
import axios from 'axios'
import moment from 'moment'
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
    amt_province: null,
    notification_news: null,
    province: []
  }),
  mounted: function() {
    this.getdata_amount_case()
    this.getdata_sum_paitaint()
    this.getdata_notification()
  },
  methods: {
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
          console.log(response.data.Province)
          for (var key in response.data.Province) {
            if (response.data.Province.hasOwnProperty(key)) {
              console.log(key + ' -> ' + response.data.Province[key])
              this.patitent_list.push({
                province: key,
                amt: response.data.Province[key]
              })
            }
          }
          // this.patitent_list = response.data
          this.amt_province = Object.values(response.data.Province).length
          // console.log(Object.values(response.data.Province).length)
        })
        .catch(function(error) {
          // handle error
          console.log(error)
        })
    },
    getdata_notification() {
      axios
        .get('https://covid19.th-stat.com/api/open/area')
        .then((response, error) => {
          console.log(response.data['Data'])
          this.notification_news = response.data['Data']
          response.data.Data.forEach((element, index) => {
            if (this.province.length == 0) {
              this.province.push({
                province_th: element.Province,
                province_en: element.ProvinceEn
              })
            } else {
              let result = this.province.find((x) => {
                return x.province_en === element.ProvinceEn
              })
              // console.log('resut arr ', result)

              if (result == undefined) {
                this.province.push({
                  province_th: element.Province,
                  province_en: element.ProvinceEn
                })
              }
            }
            // console.log('province', this.province)
            if (element.AnnounceBy == '') {
              this.notification_news[index].AnnounceBy = '-'
            }
            if (element.Time == '') {
              this.notification_news[index].Time = '-'
            }

            if (element.Update == '-') {
              this.notification_news[index]['Update'] = '-'
            } else {
              this.notification_news[index]['Update'] = moment(
                element.Update,
                'DD-MM-YYYY'
              ).format('DD-MM-YYYY')
            }
            if (element.Recommend == '-') {
              this.notification_news[index].Recommend = '-'
            } else {
              let txt_split_p = element.Recommend.split('<p>')
              let txt_split_close = txt_split_p[1].split('</p>')
              let txt_reccomend = txt_split_close[0].split('<br>')
              this.notification_news[index].Recommend = txt_reccomend[0]
            }
          })
          console.log('filter arr', this.notification_news)
          // var unique_list = new Set(this.province) // returns Set {"{'id': 123, 'name': 'ABC'}"}
          // var list = Array.from(unique_list)
          // console.log(list)
          // this.province = [...new Set([this.province])]
          // console.log(uniqueArray)
        })
        .catch(function(error) {
          console.log(error)
        })
    },
    filter_province(e) {
      var sel_val = e.target.value
      const arr_filter_cp = [...this.notification_news]
      console.log('idsel_val ', sel_val)

      if (sel_val == 'not') {
        this.getdata_notification()
      } else {
        this.notification_news = arr_filter_cp.filter(
          (x) => x.ProvinceEn === sel_val
        )
      }
    },
    filter_amt_petient(e) {
      var sel_val = e.target.value
      // this.patitent_list.Province = Object.values(this.patitent_list.Province)
      console.log(typeof this.patitent_list.Province)
      if (sel_val == 'not') {
        this.getdata_sum_paitaint()
      } else if (sel_val == 'asc') {
        this.patitent_list.sort((a, b) => a.amt - b.amt)
      } else if (sel_val == 'desc') {
        this.patitent_list.sort((a, b) => b.amt - a.amt)
      }
    }
  }
}
</script>
