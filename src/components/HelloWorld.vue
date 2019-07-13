<template>
     <div class="container">

 <!-- Register User -->
       <div v-if="!checki">
        <h3 class="m-5"> Welcome To My Goals</h3>
        <div class="row">
            <div class="card mx-auto">
                <div class="card-header text-white bg-dark">
                    <h5> My Goals </h5>
                </div>
                <div class="card-body text-left">
                    <form @submit.prevent="saveUser">
                        <div class="form-group">
                            <label for="name">Name</label>
                            <input type="text" name="name" id="name" v-model="name" placeholder="Name" class="form-control" required>
                        </div>

                        <div class="form-group">
                            <label for="dob">Date Of Birth</label>
                            <input type="date" name="dob" id="dob" v-model="dob" placeholder="DOB" class="form-control"  required>
                        </div>

                        <input type="submit" class="btn btn-primary" value="Next">
                    </form>
                </div>
            </div>
        </div>
       </div>

  <!-- User Profile If User Is Logged In -->
      <div v-if="checki">
        <profile /> 
      </div>
       
    </div>
</template>

<script>
  
import profile from './Profile'

export default {
  name: 'HelloWorld',
  components: {
    profile
  },
  data(){return{
   name: '',
   dob:'',
   user: []
  }},



  methods: {


    // Save User's Information in Localstorage 
    saveUser() {
      this.user.push({"name": this.name, "dob":this.dob});  
      localStorage.setItem("user", JSON.stringify(this.user));
      location.reload();
    },

  //Check User is logged in (check localstorage)
    checkit() {
      var x = localStorage.getItem('user')
      if(x === null)
          this.checki = false;
      else
          this.checki = true;      
   },
  
  },

  beforeMount(){
      this.checkit();     
  },

}
</script>





<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

label{
  font-size: 15px;
  margin-bottom: 0;
}
</style>
