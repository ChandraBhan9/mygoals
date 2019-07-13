<template>
        <div class="m-5">
          
          <button type="button" class="btn btn-outline-primary" data-toggle="modal" data-target="#goalModal">+ Create goal </button>
           <!--Add Goals Modal -->
            <div class="modal fade" id="goalModal" tabindex="-1" role="dialog" aria-labelledby="goalModalLabel" aria-hidden="true">
             <form @submit.prevent="saveGoal">
              <div class="modal-dialog" role="document">
                <div class="modal-content">
                  <div class="modal-header bg-dark text-white">
                    <h5 class="modal-title" id="goalModalLabel">Create goal</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body">      
                      <div class="form-group text-left">
    
                        <div class="form-row">
                          <div class="form-group col-md-7">
                            <label for="gname">Title</label>
                            <input type="gname" v-model="goals.gName" class="form-control" id="gname" placeholder="Title" required>
                          </div>
                          <div class="form-group col-md-5">
                            <label for="gdate">Due Date</label>
                            <input type="date" v-model="goals.gDate" class="form-control" id="gdate" :min="today" required>
                          </div>
                        </div>

                            <!-- <div class="form-group">
                              <label for="inputState">Tag</label>
                              <select id="inputState" v-model="goals.gTag" class="form-control fa" required >
                                  <option></option>
                                  <option v-for="(i,index) in myJson"  :value="i.class" :key="index" v-html="i.charsheet"> </option>
                                  
                            </select>
                          </div> -->

                          <div class="form-group">
                            <label for="gtag">Tag</label>
                             <input id="gtag" list="tags" v-model="goals.gTag" class="form-control fa" required> 
                                 <datalist id="tags">
                                   <option></option>
                                   <option v-for="(i,index) in myJson" :key="index">{{i.class}}</option>
                                </datalist>   
                          </div>
                        
                        <div class="form-group">
                          <label for="gdescription">Why do you want to achieve this goal?</label>
                          <textarea class="form-control" v-model="goals.gDescription" id="gdescription" required>Description of your goal...</textarea>
                        </div>

                        <div class="form-group">
                          <div class="form-check">
                            <input class="form-check-input" type="checkbox" id="gridCheck">
                            <label class="form-check-label" for="gridCheck">S.M.A.R.T Goal<br>
                                Yes, my goal is Specific, Measurable, Achievable, Relevant and Time specific.
                            </label>
                          </div>
                        </div>  
                      </div>                                   
                  </div>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">close</button>
                    <button type="submit" class="btn btn-primary">Save Goal</button>
                  </div>
                </div>
              </div>
            </form>
           </div>
         </div> 
</template>


<script>

import fontAwesomeJson from '../json/data.json'

export default {
  name: 'createGoal',

  data(){return{
   myJson: fontAwesomeJson,
   today: '',
   name: '',
   goals: {'gName': '', 'gDate': '', 'gDescription': '', 'gTag': ''},
  }},



  methods: {

    // Save User's Goals in Localstorage
    saveGoal(){
      var goals = localStorage.getItem(this.name);
      goals = goals ? JSON.parse(goals) : [];

      if(goals.length == 0) {
        goals.push(this.goals)
      }
      else { 
        for(var i=0; i<goals.length; i++){
            if( this.goals.gDate < goals[i].gDate){
            goals.splice(i,0,this.goals)
            break;
            }
            else if(i == goals.length-1){
              goals.push(this.goals);
              break;
            }
          }
        }

      localStorage.setItem(this.name, JSON.stringify(goals));
      location.reload();
    },


  //Check User is logged in (check localstorage)
    checkit: function(){
      var x = localStorage.getItem('user')
      if(x === null) return false;
      else  return true;     
   }
  
  },




  beforeMount(){
     this.checkit();
  },

  mounted(){
     if(this.checkit)
        {
            var z = localStorage.getItem('user');
            var c = JSON.parse(z);
            this.name = c[0].name

        // Date Validation for Goals
            var nowDate = new Date(); 
            var date = nowDate.getDate();
            var month = nowDate.getMonth() + 1;
            if(month < 10){ month = '0'+month}
            if(date < 10){ date = '0'+date}
            var year  = nowDate.getFullYear()
            this.today = year+'-'+month+'-'+date
                    
        }
  }
}
</script>

<style>
    
</style>
