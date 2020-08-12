<template>
  <div class="section onboarding-wizard">
    <h2>Du willst also eine Mitgliedschaft abschließen?</h2>
    <p>für <b>ermäßigt</b> bist du berechtigt bei Vorlage folgender Ausweise: 4you card, SchülerInnen, Studierende, Lehrlinge bis 28 Jahre, Behindertenpass</p>
    <form id="index" @submit="checkForm" method="post">
      <p class="alert" v-if="user.typeError">Bitte wähle welche Mitgliedschaf du haben möchtest</p>
    <div class="options">
      <div class="option" v-bind:class="{missing: user.errors.type === false}" @click="setType('regulär')">
        <input class="radio" type="radio" v-model="user.type" id="regulär" name="type" value="regulär" @click="checkTrue">
        <label class="label-big" for="regulär">regulär</label>
        </div>
      <div class="option" v-bind:class="{missing: user.errors.type === false}" @click="setType('ermäßigt')">
        <input class="radio"  type="radio" v-model="user.type" id="ermäßigt" name="type" value="ermäßigt" @click="checkTrue">
        <label class="label-big" for="ermäßigt">ermäßigt</label>
        </div>
    </div>
    <div>
      <div v-if="user.type != 'free'">
        <div class="info">
          <p><b>Tipp:</b> Bei jährlicher Zahlung bekommst du 2 Monate geschenkt.</p>
        </div>
        <p class="alert" v-if="user.periodeError">Bitte wähle welche Zahlperiode du haben möchtest</p>
        <div class="options">
          <div class="option" v-bind:class="{missing: user.errors.periode === false}">
            <input class="radio" type="radio"  v-model="user.periode" id="month" name="periode" value="month" @click="checkTrue">
            <label class="label-small" for="month">monatlich {{ price ? "40€" : "15€"}}</label>
          </div>
          <div class="option" v-bind:class="{missing: user.errors.periode === false}">
            <input class="radio" type="radio"  v-model="user.periode" id="year" name="periode" value="year" @click="checkTrue">
            <label class="label-small" for="year">jährlich {{ price ? "400€" : "150€"}}</label>
          </div>
        </div>

      </div>

    </div>
    <div class="spacer"></div>
    <div class="wizard-checkbox-agb">
      <!--<Checkbox :value="agbBool" @click="checkTrue" theme="form">Ja, ich habe die <a class="checkbox-link" :href="window+'/de/agb'" target="_blank"> Allgemeinen Nutzungsbedingungen (ANB) </a> und die <a class="checkbox-link" :href="window+'/de/faq'" target="_blank"> Werkstattordnung </a> gelesen und bin damit einverstanden.</Checkbox>

      <input type="checkbox" id="agb" name="agb" value="!agbBool" v-model="user.agbBool">
      <label for="agb">Ja, ich habe die <a class="checkbox-link" :href="window+'/de/agb'" target="_blank"> Allgemeinen Nutzungsbedingungen (ANB) </a> und die <a class="checkbox-link" :href="window+'/de/faq'" target="_blank"> Werkstattordnung </a> gelesen und bin damit einverstanden.</label><br>
   -->
    </div>
    </form>
  </div>
</template>

<script>
import Checkbox from "~/components/Checkbox.vue";

export default {
  middleware: 'authenticated',
  components: {
    Checkbox
  },
  props: ['data'],
  data () {
    return {
      loading: false,
      empty: true,
      type: 'regulär',
      periode: 'month',
      typeError: 'false',
      periodeError: 'false',
      price: true,
    }
  },
  created() {
    console.log(this.user);
    console.log('errors');
    console.log(this.user.errors);
  },
  methods: {
    checkForm() {

    },
    checkTrue() {
      let counter = 1;
      // for (let i = 0; i < counter; i++){
        console.log(this.user.type);
      console.log(this.user.periode);
      //}
    },
    setType(type) {
      this.user.type = type;
      if(type == 'ermäßigt') {
        this.price = false;
      }
      console.log(this.user);
    }
  },
  computed: {
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

.onboarding-wizard {
  height: 100%;
  display: flex;
  flex-direction: column;
  .wizard-checkbox-agb {
    width: 100%;
  }
  .checkbox-link {
    padding-left: 5px;
    padding-right: 5px;
  }
  .spacer {
    flex: 1;
  }
  .options {
    padding: 20px 0;
    display: flex;
    justify-content: space-around;
    margin: 0 -10px;

    .option {
      margin: 10px;
      flex: 1;
      cursor: pointer;
      padding: 25px;
      background-color: #FFF;
      border: 2px solid #FFFFFF;
      .label-big {
        cursor: pointer;
        padding-left: 25px;
        padding-right: 70%;
        @include media-breakpoint-down(sm) {
          padding-left: 5px;
          padding-right: 5%;
        }
      }
      .label-small {
        cursor: pointer;
        padding-left: 25px;
        padding-right: 65%;
        @include media-breakpoint-down(sm) {
          padding-left: 5px;
          padding-right: 5%;
        }
      }

      &:hover {
        border: 2px solid $color-orange;

      }
    }
  }
}
  .alert {
    color: #FF0000;
  }

</style>
