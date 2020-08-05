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
            <!--<button class="input-button-primary" v-if="index < steps.length-1" @click="next()">Weiter </button>-->
          </div>
        </div>
      </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  middleware: 'authenticated',
  data () {
    return {
      steps: ['index', 'contact', 'payment', 'done'],
      profileData: [],
      type: 'regulär',
      periode: 'monatlich',
      personalData: [],
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
      let ni = this.index + 1 < 0 ? 0 : this.index + 1;
      let path = this.steps[ni];
      this.$router.push('/wizard/onboarding/' + path);
      this.profileData.type = this.type;
      this.profileData.periode = this.periode;
      console.log(this.profileData);
      this.personalData.birthday = this.user.profile.birthdate;
      this.personalData.phone = this.user.profile.phone;
      this.personalData.address = this.user.profile.address;
      this.personalData.address2 = this.user.profile.address2;
      this.personalData.zip = this.user.profile.zip;
      this.personalData.city = this.user.profile.city;
      console.log(this.personalData);

    },
    checkForm(e){
      e.preventDefault();
    }
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
