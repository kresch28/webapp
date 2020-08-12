<template>
  <div class="wizard" v-if="user !== null">
    <div class="header">
      <h2>Mitglied werden</h2>
      <p>In 4 Schritten zu deiner <a :href="window+'/de/mitgliedschaften'" target="_blank">Mitgliedschaft</a></p>
    </div>
    <div class="wizard-section">
      <form @submit="checkForm" method="post">
      <div class="wizard-section-menu">
        <div class="steps">
          <div v-for="s,i in steps" class="step" :class="{ 'icon': index > i, 'color': index >= i}">
            <NuxtLink class="dot" v-if="i == 0" to="/wizard/onboarding/">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 230 200"><path d="M20 130l40 40L200 30" stroke-width="25" fill="none" stroke="#FFF"/></svg>
            </NuxtLink>
            <NuxtLink class="dot" v-else :to="'/wizard/onboarding/' + s">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 230 200"><path d="M20 130l40 40L200 30" stroke-width="25" fill="none" stroke="#FFF"/></svg>
            </NuxtLink>
          </div>
        </div>
      </div>
      <div class="wizard-section-content">
        <NuxtChild :key="$route.params.slug"></NuxtChild>
      </div>
      <div class="wizard-section-nav" v-if="done == false">
        <div class="form">
          <div class="button-row">
            <button class="input-button-primary" v-if="index > 0" @click="back()">Zurück</button>
            <div class="spacer"></div>
            <input class="input-button-primary" v-if="index < steps.length-1" @click="next()" type="submit" value="Weiter">
            <input class="input-button-primary" v-if="index == steps.length-1" @click="submit" type="submit" value="Abschicken">
            <!--<button class="input-button-primary" v-if="index < steps.length-1" @click="next()">Weiter </button>-->
          </div>
        </div>
      </div>
        <div v-if="done == true" class="check">
          Mitgliedschaft erflogreich abgeschlossen
          <img src="~/assets/img/icons/check-solid.svg" class="status">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  import saveAs from 'save-as'

export default {
  middleware: 'authenticated',
  data () {
    return {
      steps: ['index', 'contact', 'payment', 'done'],
      done: false,
      typeErrors: {
        'type' : '',
        'periode' : '',
        'iban' : '',
        'bank' : ''
      },
    profileData: {
        'profile_type' : '',
        'profile_period' : '',
        'firstname' : '',
        'lastname' : '',
        'birthdate' : '',
        'phone' : '',
        'street' : '',
        'street_additional' : '',
        'zip' : '',
        'city' : '',
        'country' : '',
        'iban' : '',
        'bic' : '',
      },
      profileCheck: false,
      personalData: [],
      personalCheck: false,
      payment: {},
      paymentCheck: false,
      newUser: {
        'profile' : '',
        'person' : '',
        'payment' : '',
        'file' : '',
        'picture': ''
      },
    }
  },
  created() {
    this.user.errors = this.typeErrors;
  },
  methods: {
    back() {
      let ni = this.index - 1 < 0 ? 0 : this.index - 1;
      let path = this.steps[ni];
      if (ni == 0) {
      console.log(ni);
      console.log(ni);
        path = '';
      }
      this.$router.push('/wizard/onboarding/' + path);
    },
    next() {

      console.log(this.index);

      if (this.user.type !== undefined && this.user.periode !== undefined) {
        /*if (this.user.agbBool == true && this.index < 1) {*/
        if (this.index < 1) {
          let ni = this.index + 1 < 0 ? 0 : this.index + 1;
          let path = this.steps[ni];
          this.$router.push('/wizard/onboarding/' + path);
          if(this.user.periode == 'month') {
            this.newUser = {'profile_period' : 1};
          }
          if(this.user.type == 'year') {
            this.newUser= {'profile_period' : 2};
          }
          if(this.user.type == 'regulär') {
            this.newUser.profile_type = 1;
          }
          if(this.user.type == 'ermäßigt') {
            this.newUser.profile_type = 2;
          }
          if(this.user.type == 'free') {
            this.newUser.profile_type = 3;
          }
          this.profileCheck = true;
        }
      }
      if(this.profileCheck != true && this.index == 0){
        if (this.user.type == undefined) {
          this.user.typeError = true;
          this.typeErrors.type = false;
        }
        if (this.user.periode == undefined) {
          this.user.periodeError = true;
          this.typeErrors.periode = false;
        }
        this.user.errors = this.typeErrors;
        this.profileCheck = null;
      }

      if(this.index == 1) {
        let ni = this.index + 1 < 0 ? 0 : this.index + 1;
        let path = this.steps[ni];
        this.$router.push('/wizard/onboarding/' + path);
        this.newUser.birthdate = this.user.profile.birthdate;
        this.newUser.phone = this.user.profile.phone;
        this.newUser.street = this.user.profile.address;
        this.newUser.street_additional = this.user.profile.address2;
        this.newUser.zip = this.user.profile.zip;
        this.newUser.city = this.user.profile.city;
        this.newUser.country = this.user.profile.state;
        /*this.personalData = {'birthday' : this.user.profile.birthdate};
        this.personalData.phone = this.user.profile.phone;
        this.personalData.address = this.user.profile.address;
        this.personalData.address2 = this.user.profile.address2;
        this.personalData.zip = this.user.profile.zip;
        this.personalData.city = this.user.profile.city;
        this.personalData.company = this.user.profile.company;
        this.newUser.person = this.personalData;*/
      }

      console.log('here');
      console.log(this.user.payment.iban);
      console.log(this.user.payment.bank);


      if (this.user.payment.iban !== undefined && this.user.payment.bank !== undefined) {
        if(this.user.payment.iban.length > 19 && this.user.payment.bank.length > 10){
        this.paymentCheck = false;
          /*if (this.user.sepaBool == true) {*/
            let ni = this.index + 1 < 0 ? 0 : this.index + 1;
            let path = this.steps[ni];
            console.log(ni);
            this.$router.push('/wizard/onboarding/' + path);
            /*this.payment = {'iban' : this.user.payment.iban};
            this.payment.bank = this.user.payment.bank;*/
            console.log(this.user.payment.iban.length);
            this.newUser.iban = this.user.payment.iban;
            this.newUser.bank = this.user.payment.bank;
            this.paymentCheck = true;
            /*this.newUser.payment = this.payment;*/
            console.log(this.newUser);
          //}
        }
      }

      if(this.paymentCheck != true && this.index == 2) {
        /*if(this.user.payment.iban == '' || this.user.payment.bank == '' || this.user.payment.iban.length < 20 || this.user.payment.bank.length < 11) {
          */if (this.user.payment.iban === ''){
            this.typeErrors.iban = false;
            this.user.ibanError = true;
          }
          if(this.user.payment.iban.length < 20) {
            this.typeErrors.iban = false;
            this.user.ibanError = true;
            alert('Bitte einen gültigen IBAN angeben');
          }
          if (this.user.payment.bank == ''){
            this.typeErrors.bank = false;
            this.user.bankError = true;
          }
          if(this.user.payment.bank.length < 11) {
            this.typeErrors.bank = false;
            this.user.bankError = true;
            alert('Bitte einen gültigen BIC angeben');
          }
          this.user.errors = this.typeErrors;
          this.paymentCheck = null;
        //}
      }

      /*if(this.paymentCheck != true && this.index == 2){
        if (this.user.payment.iban == '') {
          this.typeErrors.iban = false;
        }
        if (this.user.payment.bank == '') {
          this.typeErrors.bank = false;
        }
        alert('Bitte alle Felder auswählen');
        this.paymentCheck = null;
      }*/

      console.log(this.paymentCheck);

      if(this.user.file != null) {
        this.newUser.file = this.user.file;
      }

      if(this.user.picture != null) {
        this.newUser.picture = this.user.picture;
      }

      console.log(this.newUser);
      /*console.log(this.newUser);
      console.log(JSON.stringify(this.newUser));*/

    },
    checkForm(e){
      e.preventDefault();
    },
    submit(e){

      if(this.user.agbBool != true && this.index > 2) {
        alert('Hast du die ANB und die Werkstattordnung gelesen?');
      }

      if(this.user.dsBool != true && this.index > 2) {
        alert('Hast du die Datenschutzerklärung gelesen?');
        return;
      }
      if(this.user.sepaBool != true && this.index > 2) {
        alert('Bist du damit einverstanden, dass deine Mitgliedsbeiträge und zusätzlich anfallende Kosten per SEPA-Lastschrift von deinem angegeben Konto eingehoben werden?');
      }

      if(this.user.agbBool == true && this.user.dsBool == true && this.user.sepaBool == true){
          e.preventDefault();
          this.profileData.profile_type = this.newUser.profile_type;
          this.profileData.profile_period = this.newUser.profile_period;
          this.profileData.firstname = this.user.profile.firstName;
          this.profileData.lastname = this.user.profile.lastName;
          this.profileData.birthdate = this.newUser.birthdate;
          this.profileData.phone = this.newUser.phone;
          this.profileData.street = this.newUser.street;
          this.profileData.street_additional = this.newUser.street_additional;
          this.profileData.zip = this.newUser.zip;
          this.profileData.city = this.newUser.city;
          /*this.profileData.country = this.newUser.country;*/
          this.profileData.country = 'at';
          this.profileData.iban = this.newUser.iban;
          this.profileData.bic = this.newUser.bank;
          console.log(this.profileData);
          /*this.$store.dispatch("updateInvoiceContact", this.profileData).then((data) => {
            if(data) {
              this.done = true;
            }
            console.log(data);
          }).catch((err)=> {
            console.log(err);
          });*/
      }
    },
    getDocument() {
        console.log(JSON.stringify(this.profileData));
        let blob = new Blob([JSON.stringify(this.profileData)], { type: "application/json" });
        console.log(blob);
        saveAs(blob, 'onboarding.json');
        return blob;
    },
  },
  computed: {
    activeStep() {
      return this.$route.path.split('/')[3] || 'index';
    },
    index() {
      return this.steps.indexOf(this.activeStep);
    },
    user() {
      return this.$store.state.user;
    },
    window(){
      return window.location.origin
    },
  }
}
</script>

<style lang="scss">
@import '@/assets/scss/styles.scss';

.wizard {
  margin: 0 4%;
  .wizard-section {
    display: flex;
    align-items: center;
    flex-direction: column;
    @include media-breakpoint-down(sm) {
      display: block;
    }
    .wizard-section-menu {
      @include media-breakpoint-down(sm) {
        display: flex;
        justify-content: center;
      }
      .steps {
        .step {
          display: inline-block;
          position: relative;
          &:before {
            z-index: -1;
            content: '';
            position: absolute;
            margin-top: -3px;
            top: 50%;
            right: -10px;
            border-bottom: 6px solid #999;
            width: 50px;
          }
          &.color {
            .dot {
              background-color: $color-orange;
            }
          }
          &.icon {
            .dot {
              svg {
                display: block;
              }
            }
            &:before {
              border-bottom: 6px solid $color-orange;
            }
          }
          .dot {
            height: $step-size;
            width: $step-size;
            display: inline-block;
            border-radius: 50%;
            margin-right: $step-size;
            background-color: #999;
            svg {
              display: none;
              padding: 8px;
              height: 30px;
            }
          }
          &:last-child {
            .dot {
              margin-right: 0;
            }
            &:before {
              display: none;
            }
          }
        }
      }
    }
    .wizard-section-content {
      min-height: 60vh;
      min-width: 50vh;
    }
    .wizard-section-nav {
      min-width: 50vh;
      .button-row {
        display: flex;
        flex-direction: row;
        .spacer {
          flex: 1;
        }
      }
    }
  }
  .missing {
    border: 1px solid #ff0000 !important;
  }
  .missingInput {
    outline: auto;
  }
  .status {
    margin-left: 10px;
    width: 5%;
  }
  .check {
    text-align: center;
  }
}
</style>
