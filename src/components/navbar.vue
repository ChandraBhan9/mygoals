<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <router-link class="navbar-brand" to="/">Track My Goals</router-link>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav ml-auto">

      <li class="nav-item active">
       <router-link class="nav-link" to="/">{{myname}}</router-link>
      </li>
       
      <li class="nav-item active" v-if="!checki">
       <router-link class="nav-link" to="/">Register</router-link>
      </li>

      <li class="nav-item active">
       <router-link class="nav-link" to="/about">About</router-link>
      </li>

      <li class="nav-item active" v-if="checki"> 
       <a class="nav-link" v-on:click="clearData">Reset</a>
      </li>

    </ul>
  </div>
</nav>
</template>

<script>

export default {

  data(){return{
      checki: '',
      myname: ''
  }},

  methods: {
      isLoggedIn(){
      var x = localStorage.getItem('user')
      if(x === null)
      this.checki = false;
      else{
      this.checki = true
      var y = localStorage.getItem("user")
          var z = JSON.parse(y)
          this.myname = z[0].name
      }
      },

      clearData(){
          localStorage.removeItem('user')
          location.reload();
      }
  },

  created(){
      this.isLoggedIn();
  }

}
</script>

<style>

</style>
