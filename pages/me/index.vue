<template>
  <div class="section">
    <h2>Kontaktdaten</h2>
    <form class="form" @submit.prevent="updateUser">
      <div class="form-item">
        <span class="label">Vorname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.firstName" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Nachname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.lastName" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Adresse</span>
        <input class="input-text" type="text" v-model="user.profile.address" name="" id=""/>
      </div>
      <div class="form-item">
        <span></span>
        <input class="input-text" type="text" v-model="user.profile.address2" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">PLZ</span>
        <input class="input-text" type="text" v-model="user.profile.zip" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Stadt</span>
        <input class="input-text" type="text" v-model="user.profile.city" name="" id=""/>
      </div>
      <div class="button-row">
        <div v-if="loading">
          Saving…
        </div>
        <button v-else type="submit" class="input-button-primary">Speichern</button>
      </div>
    </form>
    <div>
      <h2>Dashboard</h2>
      <div v-if="courses">
        <h4 class="title">Deine Unterweisungen:</h4>
        <ul class="item-list" v-if="courses">
          <li v-for="c in courses"><!--<course :course="c" />-->
          <div class="dashboard-training-item" v-bind:class="{done : done} ">
            <div class="body">
              <div class="content">
                <span class="course-heading"><b>{{c.name}}</b></span>
              </div>
            </div>
            <div class="footer" v-if="memberCourse(c.id)">
            </div>
          </div>
          </li>
        </ul>
        <NuxtLink class="button" to="/me/trainings">Mehr</NuxtLink>
      </div>
    </div>
    <h4 class="title">Deine Aktivitäten</h4>
    <div>
      <div v-for="m, z in machines" class="dashboard-resource-info">
        <div class="dashboard-info-row">
          <span class="dashboard-resource-header"><b>{{m.name}}</b></span>
        </div>
        <div class="dashboard-info-row">
          <div class="dashboard-info-block left">
            <div class="dashboard-col log-info">
              <span class="heading">Datum - Zeit</span>
              <!--<span class="heading">Nutzung</span>
              <span class="heading">Gesamtdauer</span>-->
            </div>
          </div>
        </div>
        <div v-for="i, c in m.items" class="info-row" >
          <div class="dashboard-info-block left">
            <div class="dashboard-col log-info" v-if="c <= 0">
              <span v-if="(i.created_at.split('-')[1]) >= ('0' + new Date().getMonth())">{{i.created_at | date }} - {{i.created_at | time}}</span>
              <!--<span>{{i.active_seconds}} Sekunden</span>-->
              <!--<span>{{ i.all_seconds >= 120 ||  i.all_seconds < 60 ? (Math.round(i.all_seconds/60)) + ' Minuten' : (Math.round(i.all_seconds/60)) + ' Minute'}}</span>
            --></div>
          </div>
        </div>
        <!--</div>-->
      </div>
      <NuxtLink class="button" to="/me/log">Mehr</NuxtLink>
    </div>
  </div>
</template>

<script>
  export default {
    middleware: 'authenticated',
    data () {
      return {
        loading: false,
        done: false,

        empty : false,
        resource_name : "Maschine",
        resource_names: [],
        items: [],
        machines: [],
        machine_items :[],
      }
    },
    created() {
      this.getLogs();
    },
    methods: {
      updateUser(event) {
        this.loading = true;
        this.$store.dispatch('updateUser', Object.assign({}, this.user.profile)).then(() => {
          this.loading = false;
          this.$notify({
            title: 'Yay!',
            text: 'Änderungen gespeichert.'
          });
        }).catch((e) => {
          this.loading = false;
          this.$notify({
            title: 'Error',
            type: 'error',
            text: 'Ein Fehler ist aufgetreten.'
          });
        });
      },
      memberCourse(id) {
        if(this.$store.getters.getMemberCourseById(id).manual_activation != 0 && this.$store.getters.getMemberCourseById(id).is_valid != 0 ) {
          this.done = false;
        }
        else {
          this.done = true;
        }
        return this.$store.getters.getMemberCourseById(id);
      },
      getLogs(){
        let machineType = ["B-TEC ud-800", "CoastOne C9", "Datron MLCube", "Femi 192/M", "Formlabs Form 2 #1", "Formlabs Form 2 #2",
          "Frontdesk", "Heratherm OGS60", "Heratherm OGS750", "Kaffeemühlen", "KUKA KR 5-2 arc HW", "Lasercutter LED-Test",
          "Markforged Mark Two", "Metabo BAS261", "Pilous ARG 235 plus", "Profi Press 30 TON", "Prusa MK3S #1", "Prusa MK3S #2 + MMU2/S",
          "Prusa MK3S #3", "Sapi Elch 130", "Scantool 75X", "Schröder MHSU 2000", "Trotec Speedy 360 flexx", "Trotec Speedy 400",
          "Ultimaker 3 Dual Extrusion #1", "Ultimaker 3 Dual Extrusion #2", "Ultimaker 3 Dual Extrusion #3", "Ultimaker S5 #1", "Ultimaker S5 #2",
          "Universal Robotics UR 5e", "Voest Alpine DA 250", "Wagner Einhängekabine ID", "Walther Pilot Typ 708", "WEMO TB2024"];
        let machineName = "";
        let add = '';
        this.$store.dispatch('getRecourseLogs').then((data) =>{
          //console.log(data.data);
          for (let i = 0; i < machineType.length; i++){
            if(data.data[machineType[i]]) {
              this.resource_name = machineType[i]; // array mit namen
              this.resource_names.push(machineType[i]);
              machineName = machineType[i];
              console.log(data.data[machineName]);
              this.machines.push(
                      {"name" : machineName, "items" : data.data[machineName]}
              );
            }
          }
          for (let j=0; j < this.machines.length; j++) {
            // console.log(this.machines[j].items.length);
            for(let k = 0; k < this.machines[j].items.length; k ++) {
              // console.log(this.machines[j].items[k].created_at.split('-')[1]);
            }
          }

          this.machineType();

        }).catch((err) => {
          console.log(err);
        });
      },
      machineType(){
        for (let i = 0; i < this.machines.length; i++){
          // console.log(this.machines);
          for (let j= 0; j < this.machines[i].items.length; j ++){
            if(this.machines[i].items[j].active_seconds == this.machines[i].items[j].all_seconds){
              this.empty = true;
            }
          }
        }
        console.log(this.empty);
      },
    },
    computed: {
      user() {
        return this.$store.state.user;
      },
      memberCourses() {
        console.log(this.$store.state.memberCourses)
        return this.$store.state.memberCourses;
      },
      courses() {
        console.log(this.$store.state.courses)
        return this.$store.state.courses;
      },
    }
  }
</script>

<style lang="scss">
  @import '@/assets/scss/styles.scss';
  .dashboard-training-item {
    background-color: #FFF;
    border: 1px solid #ff8c33;
    border-radius: 5px;
    padding: 0 10px;
    @include media-breakpoint-up(sm) {
      margin: 20px 20px;
      padding: 0 10px;
      padding-top: 20px;
      width: 25%;
    }
    @include media-breakpoint-down(sm) {
      padding: 10px 20px;
    }
    .body {
      padding: 10px 0;
      color: $color-orange;
      // display: flex;
      @include media-breakpoint-down(sm) {
        display: block;
      }
      .content {
        flex: 1;
      }
      .course-info {
        padding: 10px;
        @include media-breakpoint-down(sm) {
          padding: 10px 0;
        }
      }
      .course-heading {
        padding: 10px 10px;
        @include media-breakpoint-down(sm) {
          padding: 10px 0;
        }
      }
    }
    .footer {
      font-size: 0.8em;
      padding: 5px 0;
      color: #333;
    }
  }
  .status {
    float: right;
    width: 5%;
  }
  .info {
    @include media-breakpoint-up(sm) {
      flex: 1;
      width: 25%;
    }
  }
  .input-button-primary {
    cursor: pointer;
    background-color: #ff6f00;
    color: #FFF;
    border: 1px solid #ff8c33;
    padding: 7px 12px 8px;
    line-height: 1;
    outline: none;
    align-self: center;
    margin-top: 20px;
    /*@include media-breakpoint-up(sm) {
      position: absolute;
      left: 48%;
      right: 45%;
    }
    @include media-breakpoint-down(sm) {
      position: absolute;
      left: 38%;
      right: 33%;
    }*/
  }
  .button {
    cursor: pointer;
    background-color: $color-orange;
    color: #FFF;
    min-width: 30%;
    border: 1px solid lighten($color-orange, 10);
    padding: 5px 5px;
    line-height: 1;
    outline: none;
    &:focus {
      background-color: lighten($color-orange, 10);
    }
  }

  .dashboard-training-item.done {
    background-color: #FFF;
    border: 1px solid darkgreen;
    border-radius: 5px;
    padding: 0 10px;
    @include media-breakpoint-up(sm) {
      margin: 20px 20px;
      padding: 0 10px;
      padding-top: 20px;
      width: 25%;
    }
    @include media-breakpoint-down(sm) {
      padding: 10px 20px;
    }
  }

  /*..info-row {
    font-family: $font-mono;
    font-size: 0.9rem;
    font-weight: bold;
    margin: -8px;
    display: flex;
    line-height: 1.6;*/

  .dashboard-resource-info{
    margin-top: 15px;
  }

  .dashboard-info-block {
    flex-direction: row;
    display: flex;
    .dashboard-log-info {
      margin-top: 5px;
      width: 75%;
      .heading{
        font-weight: 500;
      }
    }
    .dashboard-col {
      padding: 5px;
    }
  }
  .dashboard-info-block.left{
    flex: 2;
  }
  .dashboard-info-block.right {
    flex: 1;
  }

/*}*/

</style>
