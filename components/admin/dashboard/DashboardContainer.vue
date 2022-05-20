<template>
  <div class="pa-10">
    <v-dialog v-model="isOpen" width="1000" persistent>
      <v-card class="pa-10">
        <div align="center" class="text-h6">Track Record</div>
        <v-col cols="12" class="px-0">
          <div>Lastname</div>
          <div>
            <v-text-field outlined v-model="events.lastname"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Firstname</div>
          <div>
            <v-text-field outlined v-model="events.firstname"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Address</div>
          <div>
            <v-text-field outlined v-model="events.address"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Age</div>
          <div>
            <v-textarea outlined v-model="events.age"></v-textarea>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Gender</div>
          <div>
            <v-select
              outlined
              v-model="events.gender"
              :items="['Male', 'Female']"
            ></v-select>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Temperature</div>
          <div>
            <v-text-field outlined v-model="events.temp"></v-text-field>
          </div>
        </v-col>
         <v-col cols="12" class="px-0">
          <div>Status</div>
          <div>
            <v-select :items="['No COVID','Active','Recovered','Expired']" outlined v-model="events.status"></v-select>
          </div>
        </v-col>
        <v-card-actions>
          <v-row align="center">
            <v-col align="end">
              <v-btn color="red" text @click="isOpen = false"> Cancel </v-btn>
            </v-col>
            <v-col>
              <v-btn
                color="success"
                text
                @click="addEvents"
                :loading="buttonLoad"
              >
                Save
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <div class="text-h5">
      <b>Temperature</b>
    </div>
    <v-row class="py-10">
      <v-col align="start">
        <v-card
          class="rounded-xl"
          color="#0984e3"
          height="150"
          width="300"
          elevation="5"
          align="center"
          style="cursor: pointer"
        >
          <v-icon size="60" color="white">mdi-text-box-search-outline</v-icon>
          <div class="text-h6 white--text">
            <b>Temperature</b>
          </div>
          <div class="text-h3 white--text pt-0">
            {{ temperature }}
          </div>
        </v-card>
      </v-col>
      <!-- <v-col align="center" @click="route('donate')" >
               <v-card color="#a29bfe" height="150" width="300" elevation="2" align="center" style="cursor:pointer">
                <v-icon size="60" color="white">mdi-text-box-search-outline</v-icon>
                <div class="text-h6 white--text">
                   <b>Lorem Ipsum</b>
                </div>
                <div class="text-h3 green--text pt-5">
                 
                </div>
            </v-card>
         </v-col> -->
      <!-- <v-col align="center" @click="route('donate')" >
               <v-card height="170" width="300" elevation="5" align="center" style="cursor:pointer">
                <v-icon size="60">mdi-text-box-search-outline</v-icon>
                <div class="text-h6">
                   <b>Approved Cases</b>
                </div>
                <div class="text-h3 green--text pt-5">
                    <b>{{donation.length}}</b>
                </div>
            </v-card>
         </v-col> -->
    </v-row>
    <div>
      <v-card class="rounded-xl" elevation="5">
        <v-data-table
      class="pa-5"
      :headers="headers"
      :items="items_all"
      :loading="isLoading"
    >
     <template v-slot:[`item.status`]="{ item }">
        <div>
          <v-chip align="center" :style="getColorStatus(item.status)"
            ><span>{{ item.status }} </span></v-chip
          >
        </div>
      </template>
     <template #[`item.price`]="{ item }">
          <div>
            {{formatPrice(item.price)}}
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
      </v-card>
    </div>
  </div>
</template>

<script>
import Pusher from "pusher-js";
export default {
  created() {
    this.eventsGetall();
    // this.timestamp();
  },
  methods: {
    timestamp() {
      var today = new Date();
      var date =
        today.getFullYear() +
        "-" +
        (today.getMonth() + 1) +
        "-" +
        today.getDate();
      var time = today.getHours() + ":" + today.getMinutes();
      var dateTime = date + " " + time;

      return dateTime;
    },
    async addEvents() {
      this.buttonLoad = true;
      const response = await this.$axios
        .post(
          `/user_record`,
          {
            firstname: this.events.firstname,
            lastname: this.events.lastname,
            age: this.events.age,
            address: this.events.address,
            gender: this.events.gender,
            temp: this.events.temp,
            status: this.events.status,
          },
          {
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          }
        )
        .then(() => {
          alert('Successfully Added')
          this.buttonLoad = false;
          this.isOpen=false
        });
    },
    async eventsGetall() {
      this.isLoading = true;
      const res = await this.$axios
        .get(`/temperature`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.items_all = res.data;
          this.isLoading = false;
        });
      var pusher = new Pusher("33efacb6a0d9c7baad00", {
        cluster: "ap1",
      });
      var channel = pusher.subscribe("temperature");
      channel.bind(
        "my-test",
        function (data) {
          this.temperature = data.temp;
          this.eventsGetall();
          if (data.temp > 38) {
            this.isOpen = true;
           
          }
          this.events.temp = data.temp;
        },
        this
      );
    },
  },
  data() {
    return {
      buttonLoad: false,
      isOpen: false,
      events:{
        address:"blk1 lot 35 sterling manors subd. Anabu 1-c"
      },
      items_all: [],
      temperature: 0,
      headers: [
        { text: "ID", value: "id" },
        { text: "Date", value: "date" },
        // { text: "Alert", value: "alert" },
        // { text: "Alert Message", value: "alert_message" },
        { text: "Value", value: "temperature" },
        ,
      ],
    };
  },
};
</script>

<style>
</style>