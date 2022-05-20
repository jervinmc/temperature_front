<template>
  <div class="pa-5">
    <div>
      <div class="text-h5">
        <b>Welcome</b>
      </div>
      <div class="pb-5">
        Good Morning! We have 3 new active cases for today.
      </div>
    </div>
    <v-row>
      <v-col>
        <div>
          <v-card class="rounded-xl" height="250" elevation="5">
            <v-img class="rounded-xl" src="/people.png" height="250"></v-img>
          </v-card>
        </div>
        <div class="pt-5">
          <v-card  class="rounded-xl pa-10" elevation="5">
            <div class="text-h6">COVID-19 Cases Last 7 days</div>
            <div class="pt-5">
              The total number of COVID-19 Cases on the last 7days
            </div>
            <div class="pt-5">
              <v-row>
                <v-col>
                  <div>Total Cases</div>
                  <div style="color: #4b49ac" class="text-h5">
                    <b>{{items_all.length}}</b>
                  </div>
                </v-col>
                <v-col>
                  <div>Recovered</div>
                  <div style="color: #4b49ac" class="text-h5">
                    <b> {{recovered}}</b>
                  </div>
                </v-col>
                <v-col>
                  <div>Expired</div>
                  <div style="color: #4b49ac" class="text-h5">
                    <b>{{expired}}</b>
                  </div>
                </v-col>
              </v-row>
            </div>
             <div>
              <div>
                        <VueApexCharts type="line" height="200" :options="chartOptions" :series="series"></VueApexCharts>
              </div>
        </div>
          </v-card>
        </div>
       
      </v-col>
      <v-col>
        <v-row>
          <v-col cols="12">
            <v-card
              color="#7da0fa"
              height="120"
              class="rounded-xl pa-5"
              elevation="5"
            >
              <div class="pb-5 white--text">Total Registered Residents</div>
              <div class="text-h4 white--text">
                <b> {{items_all.length}}</b>
              </div>
            </v-card>
          </v-col>
          <v-col cols="12">
            <v-card
              color="#4747a1"
              height="120"
              class="rounded-xl pa-5"
              elevation="5"
            >
              <div class="pb-5 white--text">Total Active Cases</div>
              <div class="text-h4 white--text">
                <b>{{total_active}}</b>
              </div>
            </v-card>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-card
              color="#f3797e"
              height="120"
              class="rounded-xl pa-5"
              elevation="5"
            >
              <div class="pb-5 white--text">Recovered Residents</div>
              <div class="text-h4 white--text">
                <b> {{recovered}}</b>
              </div>
            
            </v-card>
          </v-col>
        </v-row>
        <div class="pt-5">
          <v-card class="rounded-xl pa-5" elevation="5">
            <div class="text-h6 pb-5">
              <b>COVID-19 Demographics</b>
            </div>
            <div>
              Total number of residents with COVID-19 based on AGE and GENDER
            </div>
            <!-- <div class="pt-5">
              <vue-bar-graph
                :points="[1, 4, 5, 3, 4]"
                :show-y-axis="true"
                :show-x-axis="true"
                :width="400"
                :height="200"
                :show-values="true"
                :use-custom-labels="true"
                :labels="[
                  'Jan',
                  'Fev',
                  'Mar',
                  'Abr',
                  'Mai',
                  'Jun',
                  'Jul',
                  'Ago',
                  'Set',
                  'Out',
                  'Nov',
                  'Dec',
                ]"
              />
            </div> -->
            <div>
               <div v-if="!loadData">
                      <v-row>
                        <v-col>
                          <div>
                            Age:
                          </div>
                            <VueApexCharts height="300" :options="chartOptionsPieAge" :series="pie_series_age"></VueApexCharts>
                        </v-col>
                        <v-col>
                          <div>
                            Gender:
                          </div>
                            <VueApexCharts height="300" :options="chartOptionsPieGender" :series="pie_series_gender"></VueApexCharts>
                        </v-col>
                      </v-row>
              </div>
            </div>
          </v-card>
        </div>
      </v-col>
    </v-row>
    <div>
      <v-row>
        <v-col>
          <v-card class="rounded-xl pa-5" height="400" elevation="5">
            <div class="text-h6 pa-5">
              <b>Residents List</b>
            </div>
            <div>
              <v-data-table
                class="pa-5"
                :headers="headers"
                :items="items_all"
                :loading="isLoading"
              >
                <template #[`item.price`]="{ item }">
                  <div>
                    {{ formatPrice(item.price) }}
                  </div>
                </template>
                <template v-slot:loading>
                  <v-skeleton-loader
                    v-for="n in 5"
                    :key="n"
                    type="list-item-avatar-two-line"
                    class="my-2"
                  ></v-skeleton-loader>
                </template>
                <template #[`item.opt`]="{ item }">
                  <v-menu offset-y z-index="1">
                    <template v-slot:activator="{ attrs, on }">
                      <v-btn icon v-bind="attrs" v-on="on">
                        <v-icon>mdi-dots-horizontal</v-icon>
                      </v-btn>
                    </template>
                    <v-list dense>
                      <v-list-item @click.stop="view(item)">
                        <v-list-item-content>
                          <v-list-item-title>View</v-list-item-title>
                        </v-list-item-content>
                      </v-list-item>
                    </v-list>
                  </v-menu>
                </template>
              </v-data-table>
            </div>
          </v-card>
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import VueBarGraph from "vue-bar-graph";
import VueApexCharts from "vue-apexcharts";

export default {
  computed:{
    total_active(){
     var total = this.items_all.filter(data=>data.status=='Active')
      return total.length
    },
    recovered(){
     var total = this.items_all.filter(data=>data.status=='Recovered')
      return total.length
    },
    expired(){
     var total = this.items_all.filter(data=>data.status=='Expired')
      return total.length
    },
    age1(){
     var total = this.items_all.filter(data=>data.age>6&&data.age<25)
      return total.length
    }
  },
  components: {
    VueBarGraph,
    VueApexCharts,
  },
  created(){
    this.eventsGetall()
  },
  data() {
    return {
      loadData:false,
        chartOptionsPieAge: {
          chart: {
      type: 'donut'
    },
            labels: ['1-15 years old', '16-25 years old', '26-40 years old', '41 and above']
          },
        pie_series_age: [0, 0, 0, 0],
        chartOptionsPieGender: {
          chart: {
      type: 'donut'
    },
            labels: ['Male', 'Female']
          },
        pie_series_gender: [44, 55],
        pie_labels: ['Apple', 'Mango', 'Orange', 'Watermelon'],
        series: [{
              name: "Desktops",
              data: [10, 41, 35, 51, 49, 62, 69]
          }],
          chartOptions: {
            chart: {
              height: 350,
              type: 'line',
              zoom: {
                enabled: false
              }
            },
            dataLabels: {
              enabled: false
            },
            stroke: {
              curve: 'straight'
            },
            xaxis: {
              categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul'],
            
            },
            title: {
              text: 'COVID-19 Results',
              align: 'left'
            },
            grid: {
              row: {
                colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
                opacity: 0.5
              },
            },
          },
      headers: [
        { text: "Firstname", value: "firstname" },
        { text: "Lastname", value: "lastname" },
        { text: "Gender", value: "gender" },
        { text: "Address", value: "address" },
        { text: "Age", value: "age" },
         { text: "Status", value: "status" },
        { text: "Temperature", value: "temp" },
        ,
      ],
      items_all: [],
    // options: {},
    //   series: [44, 55, 41, 17, 15]
    };
  },
  methods:{
     async eventsGetall() {
       this.loadData=true
      this.isLoading = true;
      const res = await this.$axios
        .get(`/user_record`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then((res) => {
           this.items_all = res.data;
          this.pie_series_age[0]=this.items_all.filter(data=>data.age>1&&data.age<15).length
          this.pie_series_age[1]=this.items_all.filter(data=>data.age>16&&data.age<25).length
          this.pie_series_age[2]=this.items_all.filter(data=>data.age>26&&data.age<40).length
          this.pie_series_age[3]=this.items_all.filter(data=>data.age>41).length
          this.pie_series_gender[0]=this.items_all.filter(data=>data.gender=='Male').length
          this.pie_series_gender[1]=this.items_all.filter(data=>data.gender=='Female').length
          this.loadData=false
          console.log(res.data);
         
          
          this.isLoading = false;
        });
 var pusher = new Pusher('33efacb6a0d9c7baad00', {
      cluster: 'ap1'
    });
         var channel = pusher.subscribe('temperature');
    channel.bind('my-test', function(data) {
    
         this.temperature = data.temp
         this.eventsGetall()
     if(data.temp > 30){
          this.isOpen=true
     }
     this.events.temp = data.temp
    
    },this);
    },
  }
};
</script>

<style>
</style>