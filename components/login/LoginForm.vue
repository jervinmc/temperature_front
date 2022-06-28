<template>
  <div>
    <v-snackbar
      top
      absolute
      bottom
      color="error"
      outlined
      centered
      v-model="snackbar"
    >
      Wrong Credentials
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
     <v-snackbar
      top
      absolute
      bottom
      color="error"
      outlined
      centered
      v-model="snackbarisVerified"
    >
      Not yet verified. Please check your email. Thank you!
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbarisVerified = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-card width="500">
      <!-- <div
        style="background-color: #222f3e; color: white"
        align="start"
        class="pa-5"
      >
        Login Form 
      </div> -->
      <div class="pa-5" align="start">
        <v-img src="temp_logo.png"></v-img>
        <div class="text-h6">
         Covid-19 Sterling Manors Subdivision Monitoring
        </div>
        <div>
          Sign in to continue
        </div>
        <v-row>
          <v-col>
            <div>Username</div>
            <div>
              <v-text-field outlined v-model="users.email"></v-text-field>
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
          <v-btn depressed color="#4b49ac" dark @click="login" :loading="isLoaded"> Login </v-btn>
        </div>
      </div>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      token:'',
      snackbar:false,
      img_holder: 'image_placeholder.png',
      image: '',
      url: '',
      users:[],
      isLoaded:false,
      snackbarisVerified:false
    }
  },
  methods: {
     async login() {
      this.isLoaded = true;
      var credentials = {
        email: this.users.email,
        password: this.users.password,
      };
      try {
        var response = await this.$axios
          .post("login", credentials)
          .then((response) => {
            console.log(response.data)
   
            localStorage.setItem('account_type',response.data['account_type'])
            if(response.data['status']=='400'){
                alert('Wrong Credentials')
                this.isLoaded=false;
                return
            }
             const response1 =  this.$axios
            .post(`/logs`, {
                email:this.users.email,
                account_type:response.data['account_type'],
                
            }, {
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`,
              },
            })
            .then(() => {
              this.buttonLoad = false;
                  window.location.href="/admin/dashboard"
            });

         
            
           
            
            // console.log(response)
             
            // const users = this.$axios
            //   .get(`/users/details/`, {
            //     headers: {
            //       Authorization: `Bearer ${localStorage.getItem("token")}`,
            //     },
            //   })
            //   .then((users) => {
            //     // if(!users.data.is_verified){
            //     //   this.snackbarisVerified = true
            //     //   this.isLoaded=false
            //     //   return
            //     // }
            //     localStorage.setItem("id", users.data.id);
            //     localStorage.setItem("middlename", users.data.middlename);
            //     localStorage.setItem("firstname", users.data.firstname);
            //     localStorage.setItem("lastname", users.data.lastname);
            //     localStorage.setItem("account_type",users.data.account_type)
            //     if(users.data.account_type=='Client'){
            //       window.location.href="/beneficiaries"
            //     }
            //     else{
            //         window.location.href="/admin/dashboard"
            //     }
            //     this.isLoaded = false;
                
            //     // if(users.data.is_superuser) window.location.href = "/home";
            //     // else window.location.href = "/home";
            //   });
          });

        
      } catch (error) {
          alert(error)
        this.snackbar = true;
        this.isLoaded = false;
      }
    },
    onFileUpload(e) {
      this.image = e
      e = e.target.files[0]
      if (e['name'].length > 100) {
        alert('255 characters exceeded filename.')
        return
      }
      try {
        if (e.size > 16000000) {
          alert('Only 15mb file can be accepted.')
          return
        }
      } catch (error) {
        alert(error)
        return
      }
      this.image = e
      if (e == null) {
      } else {
        this.url, (this.img_holder = URL.createObjectURL(e))
      }
    },
  },
}
</script>

<style>
</style>