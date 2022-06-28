<template>
  <div class="pa-10">
    
    <v-dialog v-model="isOpen" width="1000" persistent>
     <v-card >
      <!-- <div
        style="background-color: #222f3e; color: white"
        align="start"
        class="pa-5"
      >
        Login Form 
      </div> -->
      <div class="pa-5" align="start">
        <!-- <v-img src="/temp_logo.png" height="150" width="600" ></v-img> -->
        <div class="text-h6">
          COVID-19 Sterling Manors Subdivision Monitoring
        </div>
        <div>
        </div>
        <v-row>
          <v-col>
            <div>Email</div>
            <div>
              <v-text-field outlined v-model="users.email"></v-text-field>
            </div>
          </v-col>
          <v-col>
            <div>Account Type</div>
            <div>
              <v-select outlined :items="['Barangay Official','Barangay Captain','Dev']" v-model="users.account_type"></v-select>
            </div>
          </v-col>
          <v-col cols="12">
            <div>Password</div>
            <div>
              <v-text-field outlined v-model="users.password" type="password"></v-text-field>
            </div>
          </v-col>
        </v-row>
         <div align="center">
          <v-btn depressed color="#4b49ac" width="200"  dark @click="addEvents" :loading="buttonLoad"> Save </v-btn>
        </div>
        <div class="pt-2">

        </div>
        <div align="center">
          <v-btn depressed color="grey" width="200" dark @click="isOpen=false" :loading="buttonLoad"> Cancel </v-btn>
        </div>
       
      </div>
    </v-card>
    </v-dialog>
    <div class="text-h5">
      <b>Usermanagement</b>
    </div>
     <div align="start" v-if="account_type=='Admin'">
          <v-btn depressed color="#4b49ac" dark @click="isOpen=true" :loading="isLoaded"> Add User </v-btn>
        </div>
    <v-row class="py-10">
      <!-- <v-col align="start" @click="route('cases')">
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
      </v-col> -->
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
          `/usermanagement`,
          {
            email: this.users.email,
            password: this.users.password,
            account_type: this.users.account_type,
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
          this.eventsGetall()
        });
    },
    async eventsGetall() {
      this.account_type = localStorage.getItem('account_type')
      this.isLoading = true;
      const res = await this.$axios
        .get(`/usermanagement`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.items_all = res.data;
          this.isLoading = false;
        });
    },
  },
  data() {
    return {
      account_type:'',
        buttonLoad:false,
        users:[],
      buttonLoad: false,
      isOpen: false,
      events: [],
      items_all: [],
      temperature: 0,
      headers: [
        { text: "Email", value: "email" },
        // { text: "Alert", value: "alert" },
        // { text: "Alert Message", value: "alert_message" },
        { text: "Account Type", value: "account_type" },
        ,
      ],
    };
  },
};
</script>

<style>
</style>