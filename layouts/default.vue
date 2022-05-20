<template>
  <v-app dark>
     <v-dialog v-model="isOpenLogout" width="500" persistent>
    <v-card class="pa-10">
    <div align="center" class="text-h6">Logout User</div>
    <div align="center" class="pa-10">
        Would you like to logout?
    </div>
      <v-card-actions>
        <v-row align="center">
            <v-col align="end">
                <v-btn color="red" text @click="isOpenLogout=false"> Cancel </v-btn>
            </v-col>
            <v-col>
                <v-btn color="success" text @click="confirm"> Logout </v-btn>
            </v-col>
        </v-row>  
      </v-card-actions>
    </v-card>
  </v-dialog>
    <v-app-bar
      color="white"
      :clipped-left="clipped"
      fixed
      app
      elevation="1"
      v-if="
        
        $route.name == 'index' ||
        $route.name == 'contacts' ||
        $route.name == 'about' ||
        $route.name == 'market' ||
        $route.name == 'login' ||
        $route.name == 'seller'
      "
    >
      <v-img
        src="/logo.png"
        height="60"
        width="60"
        contain
        style="cursor: pointer"
        @click="route('index')"
      ></v-img>
      <v-spacer></v-spacer>
      <v-toolbar-title
        class="px-4 black--text"
        style="cursor: pointer; font-size: 16px"
        @click="route('index')"
        ><v-icon class="pb-1">mdi-home</v-icon>Home</v-toolbar-title
      >
      <v-toolbar-title
        v-if="token == null"
        class="px-4 black--text"
        style="cursor: pointer; font-size: 16px"
        @click="route('login')"
      >
        <v-icon class="pb-1">mdi-login</v-icon>Login</v-toolbar-title
      >
      <v-toolbar-title
        v-else
        class="px-4 black--text"
        style="cursor: pointer; font-size: 16px"
        @click="
          account_type == 'Seller'
            ? route('seller/dashboard')
            : account_type == 'Customer'
            ? route('customer/transaction')
            : route('admin/dashboard')
        "
      >
        <v-icon class="pb-1">mdi-login</v-icon>{{ name }}</v-toolbar-title
      >
      <!-- <v-toolbar-title
        v-if="token == null"
        class="px-4 black--text"
        style="cursor: pointer; font-size: 16px"
        @click="route('register')"
        ><v-icon class="pb-1">mdi-plus</v-icon> Register</v-toolbar-title
      > -->
    </v-app-bar>
    <v-app-bar  color="#222f3e" :clipped-left="clipped" fixed app elevation="1" v-else-if="account_type=='Artist'" >
    <v-app-bar-nav-icon @click="drawer=true" color="white"></v-app-bar-nav-icon>
    <v-row>
      <!-- <v-col cols="auto">
        <v-img src="/logo.png" height="60" width="60" contain style="cursor:pointer" @click="route('index')"></v-img>
      </v-col> -->
      <v-col  align-self="center">
        <div class="white--text">
         Temperature
      </div>
      </v-col>
    </v-row>
    <div class="white--text">
      {{$route.name=='admin-configuration' ? 'System Configuration' : ''}}
    </div>
    </v-app-bar>
     <v-app-bar  color="#222f3e" :clipped-left="clipped" fixed app elevation="1" v-else >
    <v-app-bar-nav-icon @click="drawer=true" color="white"></v-app-bar-nav-icon>
    <v-row>
      <!-- <v-col cols="auto">
        <v-img src="/logo.png" height="60" width="60" contain style="cursor:pointer" @click="route('index')"></v-img>
      </v-col> -->
      <v-col  align-self="center">
        <div class="white--text">
           <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-toolbar-title class="px-4 white--text" v-on="on" v-bind="attrs" style="cursor:pointer">
            <v-row>
            
              <v-col cols="auto">
                      <v-icon @click="hitNotif" color="white">mdi-bell</v-icon>
              </v-col>
                <v-col class="px-0 red--text" v-if="total_unseen!=0">
                  {{total_unseen}}
              </v-col>
            </v-row>
           </v-toolbar-title>
        </template>
              <v-list>
           <v-list-item v-for="x in items_all" :key="x">
            <v-list-item-title><v-row>
                <v-col>
                  {{x.title}}
                </v-col>
                <v-col>
                  {{x.date}}
                </v-col>
              </v-row></v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
      </div>
      </v-col>
    </v-row>
    <div class="white--text">
      {{$route.name=='admin-configuration' ? 'System Configuration' : ''}}
    </div>

    </v-app-bar>
    <v-main>
      <v-container class="pa-0" fluid>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light> mdi-repeat </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
      <v-navigation-drawer v-model="drawer" absolute bottom temporary  color="#222f3e">
         <!--eslint-disable-->
      <v-list nav dense   v-if="true">
        <v-list-item-group active-class="primary" color="white">
          <v-list-item
        
          color="white"
            :to="items[index].to"
            v-for="(key, index) in items"
            :key="index"
          > 
            <v-icon class="pr-2" color="white">{{ items[index].icon }}</v-icon>
            <v-list-item-title style="color:white">{{ items[index].title }}</v-list-item-title>
          </v-list-item>
          <v-list-item
          color="white"
          @click="logout"
          > 
            <v-icon class="pr-2" color="white">mdi-logout</v-icon>
            <v-list-item-title style="color:white">Logout</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      <v-list nav dense  v-else>
        <v-list-item-group active-class="primary" color="white">
          <v-list-item
        
          color="white"
            :to="items_artist[index].to"
            v-for="(key, index) in items_artist"
            :key="index"
          > 
            <v-icon class="pr-2" color="white">{{ items_artist[index].icon }}</v-icon>
            <v-list-item-title style="color:white">{{ items_artist[index].title }}</v-list-item-title>
          </v-list-item>
          <v-list-item
          color="white"
          @click="logout"
          > 
            <v-icon class="pr-2" color="white">mdi-logout</v-icon>
            <v-list-item-title style="color:white">Logout</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      <!-- <v-list nav dense   v-if="account_type=='Seller'">
        <v-list-item-group active-class="primary" color="white">
          <v-list-item
        
          color="white"
            :to="items_seller[index].to"
            v-for="(key, index) in items_seller"
            :key="index"
          > 
            <v-icon class="pr-2" color="white">{{ items_seller[index].icon }}</v-icon>
            <v-list-item-title style="color:white">{{ items_seller[index].title }}</v-list-item-title>
          </v-list-item>
          <v-list-item
          color="white"
          @click="logout"
          > 
            <v-icon class="pr-2" color="white">mdi-logout</v-icon>
            <v-list-item-title style="color:white">Logout</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list> -->
      <!-- <v-list nav dense   >
        <v-list-item-group active-class="primary" color="white">
          <v-list-item
        
          color="white"
            :to="items_customer[index].to"
            v-for="(key, index) in items_customer"
            :key="index"
          > 
            <v-icon class="pr-2" color="white">{{ items_customer[index].icon }}</v-icon>
            <v-list-item-title style="color:white">{{ items_customer[index].title }}</v-list-item-title>
          </v-list-item>
          <v-list-item
          color="white"
          @click="logout"
          > 
            <v-icon class="pr-2" color="white">mdi-logout</v-icon>
            <v-list-item-title style="color:white">Logout</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list> -->
    </v-navigation-drawer>
  </v-app>
</template>

<script>
import Pusher from 'pusher-js';
export default {
  name: "DefaultLayout",
  created(){
    this.account_type = localStorage.getItem('account_type')
    this.eventsGetall()
    this.pusherConnect()
  },
  data() {
    return {
      account_type:null,
      clipped: false,
      drawer: false,
      isOpenLogout:false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Home",
          to: "/admin/home",
        },
         {
          icon: "mdi-apps",
          title: "Temperature",
          to: "/admin/dashboard",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Record",
          to: "/admin/record",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Usermanagement",
          to: "/admin/usermanagement",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Logs",
          to: "/admin/logs",
        },
      ],
      items_artist: [
        {
          icon: "mdi-apps",
          title: "Design",
          to: "/artist/design",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Appointment",
          to: "/artist/appointment",
        },
      ],
      
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Vuetify.js",
      total_unseen:0,
      items_all:[]
    };
  },
  methods: {
   async hitNotif(){
     this.total_unseen=0
       await this.$axios.patch(`/notification`,{}, {
            headers: {
              Authorization:`Bearer ${localStorage.getItem("token")}`,
            },
          })
    },
     async eventsGetall() {
      const res = await this.$axios
        .get(`/notification`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.items_all = res.data;
          this.total_unseen = this.items_all.filter(data=>data.isViewed=='no')
          this.total_unseen = this.total_unseen.length
        });
    },
  pusherConnect(){
      var pusher = new Pusher('33efacb6a0d9c7baad00', {
      cluster: 'ap1'
    });
         var channel = pusher.subscribe('notification');
    channel.bind('my-test', function(data) {
    
         this.eventsGetall()
    
    },this);
    },
    logout(){
     this.isOpenLogout=true;
   },
     confirm(){
     localStorage.clear();
    window.location.href="/"
     this.isOpenLogout=false
   },
    route(name) {
      if (name == "index") {
        this.$router.push("/");
        return;
      }

      window.location.href = "/" + name;
    },
  },
};
</script>
