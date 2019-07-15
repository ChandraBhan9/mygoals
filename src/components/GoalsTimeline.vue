<template>
    <div>

 <!-- Goals Timeline  -->
       <div class="swiper-container overflow-hidden">
        <p class="text-right">
          <button type="button" class="btn btn-default btn-sm prev-slide">Prev</button>
          <button type="button" class="btn btn-default btn-sm next-slide">Next</button>
        </p>
        <div class="swiper-wrapper d-flex my-5"> 
          <div class="swiper-slide lead" v-on:click="goalModel(index)" data-toggle="modal" data-target="#exampleModalLong" v-for="(item,index) in Goals" :key="index">
            <div class="mb-2 small">
              <span class="date mx-3" v-if="item.status">Goal Achived</span>
              <span class="date" v-if="!item.status">{{updateDiffs(item.gDate, index)}} <sup>.{{time.hr}}{{time.min}}{{time.sec}}{{time.msec}}</sup></span>
            </div>
            <div  :class="{ inactive: item.status }"> <i class="fa-2x bg-white taggg" :class="item.gTag"></i> </div>
            <div class="status pt-3 position-relative">
              <span class=" font-weight-bold">{{item.gName}}</span>
            </div>
          </div>
        </div>
        <div class="swiper-pagination"></div>
      </div>

 <!-- Model For Goal Detail  -->
      <div class="modal fade" id="exampleModalLong" tabindex="-1" role="dialog" aria-labelledby="exampleModalLongTitle" aria-hidden="true">
        <div class="modal-dialog" role="document">
         <div class="modal-content">
            <div class="modal-header">
               <h5 class="modal-title" id="exampleModalLongTitle"> <i id="mytag" class="bg-white" :class="goal.gTag"></i> {{goal.gName}}</h5>
               <small class="ml-auto">Due Date : {{goal.gDate}}</small>
            </div>
            <div v-if="!goal.status" class="modal-body">
               <div>Description : {{goal.gDescription}}</div>
               <div> {{updateDiffs(goal.gDate)}}</div>
            </div>
            <div v-if="goal.status" class="modal-body">
               <h2>congratulation!!</h2>
               <div> <i class="fas text-warning fa-3x fa-award"></i></div>
               <div class="text-muted">You Achived This Goal</div>
            </div>
            <div class="modal-footer">
                <i class="fas fa-trash-alt" data-toggle="tooltip"  title="Delete Goal"  v-on:click="delGoal"></i>
               <button class="btn badge badge-pill btn-success mr-auto" v-if="!goal.status" v-on:click="achivedGoal">Mark As Achived</button>
               <button class="btn badge badge-pill btn-danger mr-auto" v-if="goal.status" v-on:click="achivedGoal">Mark As Unachived</button>
               <button type="button" class="btn btn-sm btn-secondary" data-dismiss="modal">Close</button>
               <button type="button" class="btn btn-sm btn-primary"  data-dismiss="modal" data-toggle="modal" v-if="!goal.status" data-target="#editGoal">Edit</button>
            </div>
         </div>
        </div>
     </div>

   <!-- Model For Edit Goal -->
     <div class="modal fade" id="editGoal" role="dialog" aria-labelledby="editGoal" aria-hidden="true">
        <div class="modal-dialog" role="document">
         <div class="modal-content">
            <form  @submit.prevent="editGoal">     
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLongTitle"> <i id="mytag" class="bg-white" :class="goal.gTag"></i> {{goal.gName}}</h5>
                <small class="ml-auto">Due Date : {{goal.gDate}}</small>
              </div>
                <div class="modal-body"> 
                  <div class="form-group text-left">
                    <div class="form-row">
                      <div class="form-group col-md-7">
                        <label for="gname">Title</label>
                        <input type="gname" v-model="goal.gName" class="form-control" id="gname" placeholder="Title" required>
                      </div>
                      <div class="form-group col-md-5">
                        <label for="gdate">Due Date</label>
                        <input type="date" v-model="goal.gDate" class="form-control" id="gdate" :min="today" required>
                      </div>
                    </div>
                    <div class="form-group">
                      <label for="gdescription">Why do you want to achieve this goal?</label>
                      <textarea class="form-control" v-model="goal.gDescription" id="gdescription" required>Description of your goal...</textarea>
                    </div>
                    <input type="hidden" v-model="goal.gTag">
                    <div class="form-group">
                      <div class="form-check">
                        <input class="form-check-input" type="checkbox" id="gridCheck" required>
                        <label class="form-check-label" for="gridCheck">Agree For Changes</label>
                      </div>
                    </div>  
                  </div>                                 
                </div>
            <div class="modal-footer">
               <button type="button" class="btn btn-sm btn-secondary" data-dismiss="modal">Close</button>
               <button type="submit" class="btn btn-sm btn-primary">Save Changes</button>
            </div>
            </form>
         </div>
        </div>
     </div>




    </div>
</template>


<script>

export default {
  name: 'GoalsTimeline',

  data(){return{
   Goals: [],
   today: '',
   selNum: '',
   name: '',
   checki:'',
   time: {'hr': '', 'min': '', 'sec': '', 'msec': ''},
   goal: {'gName': '', 'gTag': '', 'gDate': '', 'gDescription': '', "status": ''}
  }},



  methods: {

//Update time
    updateDiffs(gdate, index){
       var gDate= new Date(gdate);
       var today = new Date();

       var totalMonths = (gDate.getFullYear() - today.getFullYear()) * 12 + gDate.getMonth() - today.getMonth();
            var years = gDate.getFullYear() - today.getFullYear();
            if (today.getMonth() > gDate.getMonth())
                years = years - 1;
            else if (today.getMonth() === gDate.getMonth())
                if (today.getDate() > gDate.getDate())
                    years = years - 1;
        var days;
        var months;

      if (today.getDate() > gDate.getDate()) {  
          months = (totalMonths % 12);
          if (months == 0)
              months = 11;
          var x = gDate.getMonth();
          switch (x) {
              case 1: case 3: case 5: case 7: case 8: case 10:
              case 12: {
                  var a = today.getDate() - gDate.getDate();
                  days = 31 - a;
                  break;
              }
              default: {
                  var d = today.getDate() - gDate.getDate();
                  days = 30 - d;
                  break; }
          }}

      else {
          days = gDate.getDate() - today.getDate();
           months = (totalMonths % 12);
      }     
    this.time['hr'] = 24 - today.getHours();
    this.time['min'] = 60 - today.getMinutes();
    this.time['sec'] = 60 - today.getSeconds();
    var ms = 1000 - today.getMilliseconds();
    if(ms <100) this.time['msec'] = '0' + ms
         else   this.time['msec'] = ms
    
    return years + "Y "+ months + "M " + days + "D ";
   },

// Deleat Goal
    delGoal(){
      var goals = localStorage.getItem(this.name);
      goals = JSON.parse(goals);
      goals.splice(this.selNum,1)
      localStorage.setItem(this.name, JSON.stringify(goals));
      location.reload();
    },

// Show Goal detail
    goalModel(index){
      // Random Color
      var colors = ['#7797c9', '#9c77c9', '#d177b8', '#c2677c', '#c9ae69', '#5b8a43'];
      var random_color = colors[Math.floor(Math.random() * colors.length)];
      document.getElementById('mytag').style.color = random_color;
            
      this.selNum = index;
      this.goal["gName"] = this.Goals[index].gName;
      this.goal["gTag"] = this.Goals[index].gTag;
      this.goal["gDate"] = this.Goals[index].gDate;
      this.goal["gDescription"] = this.Goals[index].gDescription;
       this.goal["status"] = this.Goals[index].status;
    },
  //Edit Goal detail
    editGoal(){
      var goals = localStorage.getItem(this.name);
      goals = JSON.parse(goals);
      goals.splice(this.selNum,1)

      for(var i=0; i<goals.length; i++){
            if( this.goal.gDate < goals[i].gDate){
            goals.splice(i,0,this.goal)
            break;
            }
            else if(i == goals.length-1){
              goals.push(this.goal);
              break;
            }
          }

      localStorage.setItem(this.name, JSON.stringify(goals));
      location.reload();
    },
    //Achived Goals
      achivedGoal(){
      var goals = localStorage.getItem(this.name);
      goals = JSON.parse(goals);
      if(goals[this.selNum].status == true)
       goals[this.selNum].status = false
      else goals[this.selNum].status = true
      localStorage.setItem(this.name, JSON.stringify(goals));
      location.reload();
    },


  //Check User is logged in (check localstorage)
    checkit: function(){
      var x = localStorage.getItem('user')
      if(x === null) this.checki= false;
      else return this.checki= true;  
   },

   //Check for expired goals
   expGoals(){
       for(var n= 0; n<this.Goals.length; n++){
       if( this.today > this.Goals[n].gDate && !this.Goals[n].status){
         let removedItem =  this.Goals.splice(n,1)
          
         var Expgoals = localStorage.getItem(this.name+ ' Exp');
         Expgoals = Expgoals ? JSON.parse(Expgoals) : [];
         Expgoals.push(removedItem)
      
         localStorage.setItem(this.name+" Exp",JSON.stringify(Expgoals));
         localStorage.setItem(this.name,JSON.stringify(this.Goals));
       } }
   }

  
  },

  beforeMount(){  
          // Date Validation for Goals
          var nowDate = new Date(); 
          var date = nowDate.getDate();
          var month = nowDate.getMonth() + 1;
          if(month < 10){ month = '0'+month}
          if(date < 10){ date = '0'+date}
          var year  = nowDate.getFullYear()
          this.today = year+'-'+month+'-'+date

        this.checkit();  
         if(this.checki){
            // Set Goals Timeline
            var z = localStorage.getItem('user');
            var c = JSON.parse(z);
            this.name = c[0].name
            var mygoals = localStorage.getItem(this.name);
            this.Goals =  JSON.parse(mygoals)
          }
  },

  mounted(){  
  
       
         setInterval(() => {
          this.updateDiffs();  this.expGoals()
          },107);
      

          var swiper = new Swiper('.swiper-container', {
          //pagination: '.swiper-pagination',
          slidesPerView: 2,
          paginationClickable: true,
          grabCursor: true,
          nextButton: '.next-slide',
          prevButton: '.prev-slide',
        });  

  },
}
</script>





<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.status {
  border-top: 4px solid #3e70ff;
  bottom: 16%;
  z-index: -1;
  padding: 0px 52px;
}
  
.swiper-container {
  width: 100%;
  height: 300px;
  padding: 5px 20px 30px 20px;
}

.inactive { color: green; }

</style>

