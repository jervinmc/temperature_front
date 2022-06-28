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
            <v-select :items="['No COVID','Active','Recovered','Deceased']" outlined v-model="events.status"></v-select>
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
       <!-- <v-dialog v-model="isOpen" width="1000" persistent>
    <v-card class="pa-10">
      <div align="center" class="text-h6">Residents List</div>
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
          <v-select outlined v-model="events.gender" :items="['Male','Female']" ></v-select>
        </div>
      </v-col>
        <v-col cols="12" class="px-0">
            <div>Temperature</div>
            <div>
            <v-text-field outlined v-model="events.temp"></v-text-field>
            </div>
        </v-col>
      <v-card-actions>
        <v-row align="center">
          <v-col align="end">
            <v-btn color="red" text @click="isOpen=false"> Cancel </v-btn>
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
  </v-dialog> -->
         <div class="text-h6">
          <b>Residents List</b>
      </div>
       <div align="start">
          <v-btn depressed color="#4b49ac" dark @click="isOpen=true" :loading="isLoaded"> Add Record </v-btn>
        </div>
     <v-row class="py-10">
         <v-col align="center"  >
            <v-card color="#00cec9" height="150" width="300" elevation="2" align="center" style="cursor:pointer">
                <v-icon size="60" color="white">mdi-account-multiple</v-icon>
                <div class="text-h6 white--text">
                   <b> Active Cases</b>
                </div>
               <div class="text-h3 white--text pt-0">
                        {{total_active}}
                </div>
            </v-card>
         </v-col>
         <v-col align="center"  >
               <v-card color="#0984e3" height="150" width="300" elevation="2" align="center" style="cursor:pointer">
                <v-icon size="60" color="white">mdi-text-box-search-outline</v-icon>
                <div class="text-h6 white--text" >
                   <b>Recovered</b>
                </div>
                <div class="text-h3 white--text pt-0">
                        {{recovered}}
                </div>
            </v-card>
         </v-col>
         <v-col align="center" >
               <v-card color="#a29bfe" height="150" width="300" elevation="2" align="center" style="cursor:pointer">
                <v-icon size="60" color="white">mdi-text-box-search-outline</v-icon>
                <div class="text-h6 white--text">
                   <b>Deceased</b>
                </div>
                <div class="text-h3 white--text pt-0">
                        {{expired}}
                </div>
            </v-card>
         </v-col>
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
<v-data-table
      class="pa-5"
      :headers="headers"
      :items="items_all"
      :loading="isLoading"
    >
  
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
             <v-list-item @click.stop="view(item,'Active')">
              <v-list-item-content>
                <v-list-item-title>Active</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item @click.stop="view(item,'Recovered')">
              <v-list-item-content>
                <v-list-item-title>Recovered</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item @click.stop="view(item,'Deceased')">
              <v-list-item-content>
                <v-list-item-title>Deceased</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-menu>
      </template>
    </v-data-table>
     </div>
  </div>
</template>

<script>
import Pusher from 'pusher-js';
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
     var total = this.items_all.filter(data=>data.status=='Deceased')
      return total.length
    }
  },
    created(){
        this.eventsGetall()
    },
    methods:{
   
     async addEvents(){
         this.buttonLoad = true
            const response = await this.$axios
            .post(`/user_record`, {
                firstname:this.events.firstname,
                lastname:this.events.lastname,
                age:this.events.age,
                address:this.events.address,
                gender:this.events.gender,
                temp:this.events.temp
            }, {
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`,
              },
            })
            .then(() => {
               alert('Successfully Added')
          this.buttonLoad = false;
          this.isOpen=false
             
            });
        },
         async eventsGetall() {
      this.isLoading = true;
      const res = await this.$axios
        .get(`/user_record`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.items_all = res.data;
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
     async view(item,status) {
      this.buttonLoad = true;
      const response = await this.$axios
        .patch(
          `/user_record`,
          {
            firstname:item.firstname,
            lastname:item.lastname,
            status:status
          },
          {
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          }
        )
        .then(() => {
          alert('Successfully Updated')
          // this.buttonLoad = false;
          // this.isOpen=false
          this.eventsGetall()
        });
    },
    },
    data(){
        return {
          isLoaded:false,
          isOpen:false,
            buttonLoad:false,
            isOpen:false,
            events:[],
            items_all:[],
            temperature:0,
                headers: [
                { text: "Firstname", value: "firstname" },
                { text: "Lastname", value: "lastname" },
                { text: "Gender", value: "gender" },
                { text: "Address", value: "address" },
                { text: "Age", value: "age" },
                { text: "Status", value: "status" },
                { text: "Temperature", value: "temp" },
                { text: "Actions", value: "opt" },
                ,
            ],
        }
    }

}
</script>

<style>

</style>