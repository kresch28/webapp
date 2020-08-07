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
      <div class="wizard-section-nav">
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
      profileData: [],
      profileCheck: false,
      personalData: [],
      personalCheck: false,
      payment: [],
      paymentCheck: false,
      newUser: {
        'profile' : '',
        'person' : '',
        'payment' : '',
        'file' : ''
      },
    }
  },
  created() {
    console.log(this.profileData);
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
        if (this.user.agbBool == true && this.index < 1) {
          let ni = this.index + 1 < 0 ? 0 : this.index + 1;
          let path = this.steps[ni];
          console.log(ni);
          this.$router.push('/wizard/onboarding/' + path);
          this.profileData = {'type' : this.user.type};
          this.profileData.periode =  this.user.periode;
          this.profileCheck = true;
          this.newUser.profile = this.profileData;
          console.log(this.profileCheck);
        }
      }
      if(this.profileCheck != true && this.index == 0){
        alert('Bitte alle Felder auswählen');
      }
      if(this.user.agbBool != true && this.profileCheck != true && this.index == 0) {
        alert('Hast du die ANB und die Werkstattordnung gelesen?');
      }
      if(this.user.dsBool == true && this.index == 1) {
        let ni = this.index + 1 < 0 ? 0 : this.index + 1;
        let path = this.steps[ni];
        console.log(ni);
        this.$router.push('/wizard/onboarding/' + path);
        this.personalData = {'birthday' : this.user.profile.birthdate};
        this.personalData.phone = this.user.profile.phone;
        this.personalData.address = this.user.profile.address;
        this.personalData.address2 = this.user.profile.address2;
        this.personalData.zip = this.user.profile.zip;
        this.personalData.city = this.user.profile.city;
        this.newUser.person = this.personalData;
      }
      if(this.user.dsBool != true && this.index == 1) {
        alert('Hast du die Datenschutzerklärung gelesen?');
        return;
      }
      if (this.user.payment.iban !== undefined && this.user.payment.bic !== undefined) {
        this.paymentCheck = false;
        if (this.user.sepaBool == true) {
          let ni = this.index + 1 < 0 ? 0 : this.index + 1;
          let path = this.steps[ni];
          console.log(ni);
          this.$router.push('/wizard/onboarding/' + path);
          this.payment = {'iban' : this.user.payment.iban};
          this.payment.bic = this.user.payment.bic;
          this.paymentCheck = true;
          this.newUser.payment = this.payment;
          console.log(this.paymentCheck);
        }
      }

      if(this.user.payment.bic == undefined && this.paymentCheck != true && this.index == 2){
        alert('Bitte alle Felder auswählen');
        this.paymentCheck = null;
      }

      console.log(this.paymentCheck);
      if(this.user.sepaBool != true && this.paymentCheck != true && this.paymentCheck != null && this.index == 2) {
        alert('Bist du damit einverstanden, dass deine Mitgliedsbeiträge und zusätzlich anfallende Kosten per SEPA-Lastschrift von deinem angegeben Konto eingehoben werden?');
      }

      if(this.user.file != null) {
        this.newUser.file = this.user.file;
      }

      console.log(this.newUser);
      console.log(JSON.stringify(this.newUser));

    },
    checkForm(e){
      e.preventDefault();
    },
    submit(e){
      e.preventDefault();
      console.log(this.newUser);
      this.getDocument();
    },
    getDocument() {
        console.log(this.newUser);
        console.log(JSON.stringify(this.newUser));
        let blob = new Blob([JSON.stringify(this.newUser)], { type: "text/plain" });
        console.log(blob);
        saveAs(blob, 'onboarding.json');
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
    .wizard-section-menu {
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
}
</style>
