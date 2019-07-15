<template>
     
  <div>

  <!-- User Profile -->  
         <div class="p-5">
            <h1><span class="text-secondary" style="font-size:20px">Age</span> {{years}}<sup>.{{hours}}{{minutes}}{{seconds}}{{mseconds}}</sup> </h1>
         </div>

  <!-- Create Goals Component -->
         <createGoal />

  <!-- Modal For Goals Detail -->
         <Timeline />

   </div>
</template>

<script>

import createGoal from './CreateGoal'
import Timeline from './GoalsTimeline'

export default {
  name: 'HelloWorld',
  components: {
   createGoal,
   Timeline
  },
  data(){return{
    mydob: "",
    checki: "",
    years: '',
    hours:0,
    minutes:0,
    seconds:0,
    mseconds:0,
  }},

  methods: {

  //Check User is logged in (check localstorage)
    checkit: function(){
      var x = localStorage.getItem('user')
      if(x === null)
          this.checki = false;

      else
        { 
          this.checki = true;  
          var y = localStorage.getItem("user")
          var z = JSON.parse(y)
          this.mydob = z[0].dob     
        }
   },

 //Calculate age in year,month and day
    updateDiffs() {
       
        var today = new Date();
        var DOB = new Date(this.mydob);

        var totalMonths = (today.getFullYear() - DOB.getFullYear()) * 12 + today.getMonth() - DOB.getMonth();
            totalMonths += -1;
            var years = today.getFullYear() - DOB.getFullYear();
            if (DOB.getMonth() > today.getMonth())
                years = years - 1;
            else if (DOB.getMonth() === today.getMonth())
                if (DOB.getDate() > today.getDate())
                    years = years - 1;
         var days;
         var months;
  
  
      if (DOB.getDate() > today.getDate()) {  
          months = (totalMonths % 12);
          if (months == 0)
              months = 11;
          var x = today.getMonth();
          switch (x) {
              case 1: case 3: case 5: case 7: case 8: case 10:
              case 12: {
                  var a = DOB.getDate() - today.getDate();
                  days = 31 - a;
                  break;
              }
              default: {
                  var d = DOB.getDate() - today.getDate();
                  days = 30 - d;
                  break;
              }
          }
      }
      else {
          days = today.getDate() - DOB.getDate();
          if (DOB.getMonth() === today.getMonth())
              months = (totalMonths % 12);
          else
              months = (totalMonths % 12) + 1;
      }     
        this.hours =  today.getHours();
        this.minutes = today.getMinutes();
        this.seconds =  today.getSeconds();
        var ms = today.getMilliseconds();
         if(ms <100) this.mseconds = '0' + ms
         else  this.mseconds = ms
        this.years = years
    }
  
  },

  beforeMount(){
      this.checkit();
  },

  mounted() {
    var interval = setInterval(() => {
      this.updateDiffs();
    },107);
     this.updateDiffs();
  },

}
</script>
