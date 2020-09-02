<template>
  <div class="section">
    <div class="dashboard-member">
      <div class="dashboard-member-item">
        <h2>Dashboard</h2>
        <div v-if="packages" class="packages">
          <h4 class="title">Deine Paket:</h4>
          <ul class="item-list" v-if="packages">
            <li><!--<course :course="c" />-->
              <div class="dashboard-package-item">
                <div class="body">
                  <div class="content">
                    <span class="package-heading"><b>{{package}}</b></span>
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </div>
        <h4 class="title">Meine Rechnungen</h4>
        <div class="invoices">
          <span v-if="name == null" class="noInvoice">Hier kannst du bald eine Übersicht deiner Rechnungen bekommen</span>
          <div v-if="name != null" class="invoices-info">
            <div class="info-row">
              <div class="info-block">
                <div class="col info">
                  <span v-if="name != null" class="invoice-header">{{name}}</span>
                  <span v-if="month != null">{{month}} /</span>
                  <span v-if="year != null">{{year}}</span>
                </div>
              </div>
            </div>
            <div class="info-row">
              <div class="info-block">
                <div class="col info">
                  <span>Nr: {{number}}</span>
                </div>
                <div class="col info">
                  Status: <span v-bind:class="{ paid: status_id == 4 || status_id == 5}">{{status}}</span>
                </div>
                <div class="col info">
                  <span>Datum: {{date | date}}</span>
                </div>
              </div>

            </div>
          </div>
          <div class="link-container">
            <NuxtLink class="button" to="/me/invoices">Mehr</NuxtLink>
          </div>
        </div>

      </div>
      <div class="dashboard-member-item right" v-if="machines.length > 0">
        <div v-if="courses" class="courses">
          <h4 class="title">Deine Unterweisungen:</h4>
          <ul class="item-list" v-if="courses">
            <li v-for="c in courses"><!--<course :course="c" />-->
              <div class="dashboard-training-item" v-bind:class="[$store.getters.getMemberCourseById(c.id).manual_activation != 0 && $store.getters.getMemberCourseById(c.id).is_valid != 0 ? 'done' : '']">

                <div class="body">
                  <div class="content">
                    <span class="course-heading"><b>{{c.name}}</b></span>
                  </div>
                </div>
              </div>
            </li>
          </ul>
          <div class="link-container">
            <NuxtLink class="button" to="/me/trainings">Mehr</NuxtLink>
          </div>
        </div>
        <h4 class="title">Deine Aktivitäten</h4>
        <div class="dashboard-activities">
          <!--<div v-for="m, z in machines" class="dashboard-resource-info">-->

            <!--<div class="dashboard-info-row">
              <div class="dashboard-info-block left">
                <div class="dashboard-col log-info">
                  <span class="heading">Datum - Zeit</span>
                  <span class="heading">Nutzung</span>
                  <span class="heading">Gesamtdauer</span>
                </div>
              </div>
            </div>-->
            <div v-for="i, c in items" class="dashboard-info-row" v-if="c >= (items.length - 5)">
              <span class="dashboard-resource-header"><b>{{i.resource_name}}</b></span>
              <div class="dashboard-info-block left"> <!--v-if="(i.created_at.split('-')[1]) >= ('0' + new Date().getMonth())"-->
                <div class="log-info">
                  <span v-if="c >= (items.length - 5)">{{i.created_at | date }} - {{i.created_at | time}}</span>
                  <!--<span>{{i.active_seconds}} Sekunden</span>-->
                  <!--<span>{{ i.all_seconds >= 120 ||  i.all_seconds < 60 ? (Math.round(i.all_seconds/60)) + ' Minuten' : (Math.round(i.all_seconds/60)) + ' Minute'}}</span>
                --></div>
              </div>
              <!--</div>
              </div>-->
          </div>
        </div>
        <div class="link-container">
          <NuxtLink class="button" to="/me/log">Mehr</NuxtLink>
        </div>
      </div>
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
        itemsSorted : [],
        machines: [],
        machine_items :[],
        package: '',

        name: null,
        number: "AR-00000",
        status_id: 0,
        status: "In Bearbeitung",
        date: null,
        month: null,
        year: null,
      }
    },
    created() {
      this.getLogs();
      this.user;
      this.getInvoices();
    },
    methods: {
      getInvoices() {
        let statusDescription = ["In Bearbeitung", "Bestellt", "Versendet", "Bezahlt", "Bezahlt", "Abbuchung wurde noch nicht durchgeführt", "Noch nicht Bezahlt"];
        this.$store.dispatch("getInvoices").then((data) => {
          for(let i = 0; i < data.data.length; i++){
            this.name = data.data[i].name;
            this.date = data.data[i].due_date;
            if(data.data[i].name == "member.auto-invoice.name"){
              this.name = "Mitgliedschaft"
              this.year = data.data[i].due_date.split('-')[0];
              this.month = data.data[i].due_date.split('-')[1];
            }
            this.number = data.data[i].human_readable_id;
            this.status_id = data.data[i].status;
            for (let j = 0; j < statusDescription.length; j++){
              if(data.data[i].status == j){
                console.log(statusDescription[j]);
                this.status = statusDescription[j-1];
              }
            }
            this.url = data.data[i].url.split('/')[6];
            console.log(this.url);
          }
        }).catch((err) => {
          console.log(err);
        });
      },
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
      getLogs(){
        let machineType = ["B-TEC ud-800", "CoastOne C9", "Datron MLCube", "Femi 192/M", "Formlabs Form 2 #1", "Formlabs Form 2 #2",
          "Frontdesk", "Heratherm OGS60", "Heratherm OGS750", "Kaffeemühlen", "KUKA KR 5-2 arc HW", "Lasercutter LED-Test",
          "Markforged Mark Two", "Metabo BAS261", "Pilous ARG 235 plus", "Profi Press 30 TON", "Prusa MK3S #1", "Prusa MK3S #2 + MMU2/S",
          "Prusa MK3S #3", "Sapi Elch 130", "Scantool 75X", "Schröder MHSU 2000", "Trotec Speedy 360 flexx", "Trotec Speedy 400",
          "Ultimaker 3 Dual Extrusion #1", "Ultimaker 3 Dual Extrusion #2", "Ultimaker 3 Dual Extrusion #3", "Ultimaker S5 #1", "Ultimaker S5 #2",
          "Universal Robotics UR 5e", "Voest Alpine DA 250", "Wagner Einhängekabine ID", "Walther Pilot Typ 708", "WEMO TB2024"];
        let machineName = "";
        let a;
        let b;
        this.$store.dispatch('getRecourseLogs').then((data) =>{
          // console.log(data.data);
          for (let i = 0; i < machineType.length; i++){
            if(data.data[machineType[i]]) {
              this.resource_name = machineType[i]; // array mit namen
              this.resource_names.push(machineType[i]);
              machineName = machineType[i];
              this.machines.push(
                      {"name" : machineName, "items" : data.data[machineName]}
              );
            }
          }
          for (let j=0; j < this.machines.length; j++) {
              this.items.push(this.machines[j].items[0]);
          }

          for(let k = 0; k < this.items.length; k++){
            a = this.items[k];
            b = this.items[k+1]

            this.itemsSorted = this.items.sort(function compare(a, b) {
              if (new Date(a.created_at) < new Date(b.created_at))
                return -1;
              if (new Date(a.created_at) > new Date(b.created_at))
                return 1;
              return 0;
            });
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
        /*console.log(this.empty);*/
      },

      getUser() {
        this.$store.dispatch('getUser').then((data) =>{
          console.log(data);
        })
      }
    },
    computed: {
      user() {
        console.log(this.$store.state.user);
        return this.$store.state.user;
      },
      memberCourse(id) {
        /*console.log(id);
        console.log(this.$store.getters.getMemberCourseById(id).manual_activation);
        console.log(this.$store.getters.getMemberCourseById(id).is_valid);*/
        if(this.$store.getters.getMemberCourseById(id).manual_activation != 0 && this.$store.getters.getMemberCourseById(id).is_valid != 0 ) {
          this.done = false;
        }
        else {
          this.done = true;
        }
        // return this.done;
      },
      packages(){
        if(this.$store.state.user.packages.length > 0) {
          if ((this.$store.state.user.packages[0].recurringFee == '15.00') && (this.$store.state.user.packages[0].recurringFeePeriod == 'month')) {
            console.log('Basic Garage (ermäßigt)');
            this.package = 'Basic Garage (ermäßigt)';
          }
          if ((this.$store.state.user.packages[0].recurringFee == '40.00') && (this.$store.state.user.packages[0].recurringFeePeriod == 'month')) {
            console.log('Basic Garage');
            this.package = 'Basic Garage';
          }
          if ((this.$store.state.user.packages[0].recurringFee == '0') && (this.$store.state.user.packages[0].recurringFeePeriod == 'month')) {
            console.log('Basic Garage (FREE)');
            this.package = 'Basic Garage (FREE)';
          }
          if ((this.$store.state.user.packages[0].recurringFee == '400') && (this.$store.state.user.packages[0].recurringFeePeriod == 'year')) {
            console.log('Basic Garage (jährlich)');
            this.package = 'Basic Garage (jährlich)';
          }
          if ((this.$store.state.user.packages[0].recurringFee == '150') && (this.$store.state.user.packages[0].recurringFeePeriod == 'year')) {
            console.log('Basic Garage (jährlich ermäßigt)');
            this.package = 'Basic Garage (jährlich ermäßigt)';
          }
          if ((this.$store.state.user.packages[0].recurringFee == '150') && (this.$store.state.user.packages[0].recurringFeePeriod == 'year')) {
            console.log('Basic Garage (jährlich ermäßigt)');
            this.package = 'Basic Garage (jährlich ermäßigt)';
          }
        }
        return this.package;
      },
      memberCourses() {
        return this.$store.state.memberCourses;
      },
      courses() {
        return this.$store.state.courses;
      },
    }
  }
</script>

<style lang="scss">
  @import '@/assets/scss/styles.scss';

  .dashboard-member {
    @include media-breakpoint-up(sm){
      width: 75%;
      margin-left: auto;
      margin-right: auto;
    }

    .dashboard-member-item.right{
     margin-top: 49px;
    }

      .dashboard-member-item {
      flex: 1;

      .title {
        border-bottom: 1px solid lightgray;
        padding: 10px 0;
        text-align: center;
        margin: 40px 10px;
      }

      .courses {
        display: flex;
        flex-direction: column;

        .item-list li {
          @include media-breakpoint-down(sm) {
            padding: 10px 10px;
          }
        }
      }

      .dashboard-activities {
        @include media-breakpoint-up(sm) {
          display: flex;
          flex-direction: column;
          align-items: center;
        }
        .dashboard-info-row {
          margin: 10px;
          background-color: white;
          padding: 10px 20px;
          border: 1px solid red;
          border-radius: 5px;
        }
        .dashboard-resource-info{
          margin: 15px;
        }
      }

      .dashboard-training-item {
        background-color: #FFF;
        border: 1px solid #ff8c33;
        border-radius: 5px;
        padding: 0 10px;
        @include media-breakpoint-up(sm) {
          margin: 20px 0px;
          padding: 10px 10px;
          width: 50%;
          margin-left: auto;
          margin-right: auto;
        }
        @include media-breakpoint-down(sm) {
          padding: 10px 20px;
        }

        .body {
          padding: 10px 0;
          color: $color-orange;
          @include media-breakpoint-down(sm) {
            display: block;
          }

          .course-heading {
            padding: 10px 10px;
            @include media-breakpoint-down(sm) {
              padding: 10px 0;
            }
          }
        }
      }

      .dashboard-training-item.done {
        background-color: #FFF;
        border: 1px solid darkgreen;
        border-radius: 5px;
        padding: 0 10px;
        @include media-breakpoint-up(sm) {
          margin: 20px 0px;
          padding: 10px 10px;
          width: 50%;
          margin-left: auto;
          margin-right: auto;
        }
        @include media-breakpoint-down(sm) {
          padding: 10px 20px;
        }
      }

      .link-container {
        display: flex;
        justify-content: center;
        margin-top: 15px;
        .button {
          cursor: pointer;
          background-color: $color-orange;
          color: #FFF;
          border: 1px solid lighten($color-orange, 10);
          padding: 5px 10px;
          line-height: 1;
          outline: none;
          text-align: center;
          &:focus {
            background-color: lighten($color-orange, 10);
          }
        }
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
      }
      .dashboard-info-block.left{
        flex: 2;
      }
      .dashboard-info-block.right {
        flex: 1;
      }

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
  }

  .invoices {
    width: 100%;
    margin-top: 20px;
    .noInvoice{
      @include media-breakpoint-up(sm) {
        display: flex;
        justify-content: center;
      }
    }
    .invoices-info {
      margin-top: 4px;
      padding: 10px;
      width: 75%;
      margin-left: auto;
      margin-right: auto;
      background-color: #FFF;
      @include media-breakpoint-down(sm){
        width: 100%;
      }
      &.soldOut {
        color: #666;
        fill: #666;
        .col {
          &.info {
            text-decoration: line-through;
          }
        }
      }
      .info-row {
        @include media-breakpoint-down(md) {
          flex-direction: column;
        }
        line-height: 1.6;
        font-family: $font-mono;
        font-size: 0.9rem;
        margin: -8px;
        display: flex;
        .info-block {
          flex: 1;
          flex-direction: row;
          display: flex;
        }
        .col {
          padding: 8px;
          &.soldOut {
            color: $color-orange;
            text-transform: uppercase;
          }
          &.register {
            background-color: $color-orange;
            a {
              color: #FFF;
            }
          }
        }
        .spacer {
          flex: 1;
        }
        svg {
          height: 1em;
          width: 1em;
        }
      }
    }
    .paid {
      color: #90ee90;
    }
    .invoice-header {
      font-size: large;
      font-weight: 900;
    }
  }

  .dashboard-info-row {
    font-family: $font-mono;
    font-size: 0.9rem;
    font-weight: bold;
    margin: -8px;
    line-height: 1.6;
    .dashboard-resource-header{
      color: #ff6f00;
      font-size: large;
      font-weight: 700;

    }

  }


</style>
