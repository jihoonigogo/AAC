<template>
  <v-container class="home" fluid>
    <video autoplay muted loop class="mainvideo" >
      <source src="../assets/blackwidow.mp4" type="video/mp4"/>
    </video>
      <v-content>
      <v-row>
        <v-col cols="12">
          <v-sheet
          class="netRec ma-3"
          elevation="300"
          width="90vw"
          dark
          >
          <h1 class="font-weight-black" style="font-family:none;"></h1>
          <v-slide-group
            class=""
            show-arrows
          >
            <v-slide-item
              class="card-item"
              v-for="(slide,i) in slides"
              :key="slide"
              v-slot="{ active, toggle }"
            >
              <v-card
              :color="active ? 'red' : 'black'"
              class="ma-5 cards"
              width="30vh"
              @click="toggle"
              to="/"
              hover
              >
              <h1 style="right:-10%; color:transparent;">{{i+1}}</h1>
              <v-img
                class="white--text align-end"
                width="22vh"
                height="40vh"
                :src="slide.img"
              >
                <!-- <v-card-title class="cards-title">{{slide.title}}</v-card-title> -->
              </v-img>
            </v-card>
              </v-slide-item>
            </v-slide-group>
          </v-sheet>
        </v-col>
      </v-row>
    </v-content>

    <!-- 통계 -->
    <v-row justify="" class="mb-0 pa-8 sta">
      <v-col sm="12" md="4">
        <v-card dark style="background:#202936" height="70vh">
          <v-list style="background:#202936">
            <v-subheader>POPULAR STREAMING MOVIES</v-subheader>
            <v-subheader><small>source of Rotten tomatoes</small></v-subheader>

            <v-list-item-group
              color="primary"
            >
              <v-list-item
                v-for="(item, i) in movieRank"
                :key="i"
              >
                <v-list-item-icon>
                  <v-icon>mdi-movie-open</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title v-text="item.popular"></v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
      <v-col  sm="12" md="4">
        <v-card dark style="background:#202936">
          <v-card-title>{{$t('home.marketShare')}}</v-card-title>
          <v-card-subtitle>{{$t('home.unit')}}</v-card-subtitle>
          <doughnut-chart :chart-data="doughnutCollection" :options="optionss"></doughnut-chart>
        </v-card>
      </v-col>
      <v-col  xs="12" sm="12" md="4">
        <v-card dark style="background:#202936">
          <v-card-title>{{$t('home.usageFee')}}</v-card-title>
          <v-card-subtitle>{{$t('home.usageFeeUnit')}}</v-card-subtitle>
          <bar-chart :chart-data="barchartdata" :options="options"></bar-chart>
        </v-card>
      </v-col>
    </v-row>
  </v-container>



</template>

<script>
import DoughnutChart from './DoughtnutChart.vue'
import BarChart from './BarChart.vue'
import axios from 'axios'
export default {
  components:{
    DoughnutChart,
    BarChart,
  },
  mounted () {
    this.list()
    let lang = localStorage.getItem('language')
              if (lang == "한국어"){
                  this.$i18n.locale="한국어"
              } else if (lang == "English"){
                  this.$i18n.locale="English"
              } else {
                  this.$i18n.locale="日本語"
              }

  },
  data () {
    return {
      movieRank: [],
      langs: ['ko', 'en'],
      items: [
        { text: 'Real-Time', icon: 'mdi-numeric-1-box' },
        { text: 'Audience', icon: 'mdi-numeric-2-box' },
        { text: 'Conversions', icon: 'mdi-numeric-3-box' },
      ],
      doughnutCollection: {
      labels: ["Netflix", "Wavve", "TVing", "Seezn", "WATCHA"],
      datasets: [
        {
          borderWidth: 5,
          backgroundColor: ["#E50914", "#011FFD", "#D81B60", "#FFC300", "#FF5733"],
          hoverBackgroundColor: ["#E50914", "#011FFD", "#D81B60", "#FFC300", "#FF5733"],
          hoverBorderColor: ["#E50914", "#011FFD", "#D81B60", "#FFC300", "#FF5733"],
          data: [1000, 395, 265, 168, 139]
        }
      ],
      },
      slides: [
          {title:'베놈2', to:"" ,syno:'blabla... ',img:'https://cdn.eyesmag.com/content/uploads/posts/2021/02/17/disney-cruella-new-poster-emma-stone-1-d28a46b1-8f9f-49ab-b405-7efaf62d55c2.jpg'},
          {title:'크루엘라', to:"" ,syno:'blabla',img:'https://cdn.ilyoseoul.co.kr/news/photo/202004/384931_301204_3429.jpg'},
          {title:'귀멸의칼날', to:"" ,syno:'blabla',img:'https://cdn.eyesmag.com/content/uploads/posts/2021/02/17/disney-cruella-new-poster-emma-stone-1-d28a46b1-8f9f-49ab-b405-7efaf62d55c2.jpg'},
          {title:'컨져링3', to:"" ,syno:'blabla',img:'https://www.sports-g.com/wp-content/uploads/2018/12/%EC%98%81%ED%99%94-%EC%8A%A4%ED%8C%8C%EC%9D%B4%EB%8D%94%EB%A7%A83-%ED%8F%AC%EC%8A%A4%ED%84%B0.jpg'},
          {title:'콰이어트플레이스2', to:"" ,syno:'blabla',img:'https://img.hankyung.com/photo/202106/01.26629700.1.jpg'},
          {title:'오늘점심', to:"" ,syno:'돈까스',img:'https://t1.daumcdn.net/thumb/R720x0.fjpg/?fname=http://t1.daumcdn.net/brunch/service/user/6idc/image/XIuGoXWxOpHLJpctmWaRW7g2MKc.jpeg'},
          {title:'베놈2', to:"" ,syno:'blabla... ',img:'https://cdn.eyesmag.com/content/uploads/posts/2021/02/17/disney-cruella-new-poster-emma-stone-1-d28a46b1-8f9f-49ab-b405-7efaf62d55c2.jpg'},
         ],
      optionss:{
        plugins: {
                doughnutlabel: {
                  labels: [
                    {
                      text: 'Market',
                      font: {
                        size: '30',
                        weight: 'bold',
                      }
                    },{
                      text: 'Share',
                      font: {
                        size: '20',
                        weight: 'bold',
                      }
                    },
                  ],
                },
              },
        },
      barchartdata:{
        labels: ["Netflix", "Wavve", "TVing", "Seezn", "WATCHA"],
        datasets:[
          {
            borderWidth: 5,
            backgroundColor: ["#E50914", "#011FFD", "#D81B60", "#FFC300", "#FF5733"],
            hoverBackgroundColor: ["#E50914", "#011FFD", "#D81B60", "#FFC300", "#FF5733"],
            hoverBorderColor: ["#E50914", "#011FFD", "#D81B60", "#FFC300", "#FF5733"],
            data: [95, 79, 79, 55, 79]
          },
        ]
      },
      options: {
        title: {
          display:false
        },
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: true
            }
          }]
        },
        legend: {
          display: false
        },
      }


    }
  },
  methods:{
    list () {
      console.log("start")
        axios.get('/api/movierank/list/rank')
          .then(({ data }) => {
            console.log(data)
            this.movieRank = data.ds
          })
          .catch((e) => {
            this.$store.commit('pop', { msg: e.message, color: 'warning' })
          })
    },

  }
}
</script>

<style scoped>

@media screen and (min-width:960px) {


.mainvideo{
  position: relative;
  max-height: 90vh;
  width: 100%;
  object-fit: fill;
  overflow: hidden;
}
.container {
  padding: 0;
  background: #000C1D;
}
.netRec{
  background-color: #101A29;
  /* border: solid 1px white; */
  margin-bottom: 10px;
  padding: 0px;
  top: -10%;
}
.card-item{
    margin: 30px;
    transform-style: preserve-3d;
    perspective: 700px;
    animation: spin 3s infinite linear;
}
.cards{
    display: flex;
    margin: 30px;
    transform-style: preserve-3d;
    perspective: 700px;
    animation: spin 5s infinite linear;
    /* background-color: black; */
    margin: 0px;
    padding: 0px;
}
.cards h1{
    font-size: 12em;
    font-family: 'Babas Neue' !important;
    width: 140px;
    height: 200px;
    margin:0px;
    transform: translateZ(1px);
    position: relative;
    -webkit-text-stroke: grey 3px;
}
@keyframes spin {
   from{
    transform: rotateY(-20deg);
  }to{
    transform: rotateY(20deg);
  }
}

.mainvideo{
  position: relative;
  max-height: 90vh;
  width: 100%;
  object-fit: fill;
  overflow: hidden;
}

.card1{
  height: 550px;
}
}

/* 분기점 */
@media screen and (max-width:768px) {


.mainvideo{
  position: relative;
  max-height: 90vh;
  width: 100%;
  object-fit: fill;
  overflow: hidden;
  /* min-height: 40vh; */
}
.container {
  padding: 0;
  background: #000C1D;
}
.netRec{
  /* background-color: #101A29; */
  background-color: gray;
  /* border: solid 1px white; */
  /* margin-bottom: 10px; */
  padding: 0px;
  margin: 0px;
  /* top: -10%; */
}
.card-item{
    /* margin: 30px; */
    transform-style: preserve-3d;
    perspective: 700px;
    animation: spin 3s infinite linear;
}
.cards{
    display: flex;
    /* margin: 30px; */
    transform-style: preserve-3d;
    perspective: 700px;
    animation: spin 5s infinite linear;
    /* background-color: black; */
    margin: 0px;
    padding: 0px;
}
.cards h1{
    font-size: 8em;
    font-family: 'Babas Neue' !important;
    width: 140px;
    height: 200px;
    margin:0px;
    transform: translateZ(1px);
    position: relative;
    -webkit-text-stroke: grey 3px;
}
@keyframes spin {
   from{
    transform: rotateY(-20deg);
  }to{
    transform: rotateY(20deg);
  }
}

.mainvideo{
  position: relative;
  max-height: 90vh;
  width: 100%;
  object-fit: fill;
  overflow: hidden;
}

.card1{
  height: 550px;
}
}

</style>
