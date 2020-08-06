<template>
  <div class="section onboarding-wizard">
    <h2>Du willst also eine Mitgliedschaft abschließen?</h2>
    <p>für <b>ermäßigt</b> bist du berechtigt bei Vorlage folgender Ausweise: 4you card, SchülerInnen, Studierende, Lehrlinge bis 28 Jahre, Behindertenpass</p>
    <form id="index" @submit="checkForm" method="post">
    <div class="options">
      <div class="option">
        <input class="radio" type="radio"  v-model="user.type" id="regulär" name="type" value="regulär">
        <b>regulär</b>
        </div>
      <div class="option">
        <input class="radio" type="radio" v-model="user.type" id="ermäßigt" name="type" value="ermäßigt">
        <b>ermäßigt</b>
        </div>
      <div class="option">
        <input class="radio" type="radio" v-model="user.type" id="free" name="type" value="free">
        <b>free</b>
        </div>
    </div>
    <div>
      <div v-if="user.type != 'free'">
        <div class="info">
          <p><b>Tipp:</b> Bei jährlicher Zahlung bekommst du 2 Monate geschenkt.</p>
        </div>
        <div class="options">
          <div class="option">
            <input class="radio" type="radio"  v-model="user.periode" id="month" name="periode" value="month">
            <b>monatlich {{ user.type == 'regulär' ? "40€" : "15€"}}</b>
          </div>
          <div class="option">
            <input class="radio" type="radio"  v-model="user.periode" id="year" name="periode" value="year">
            <b>jährlich {{ user.type == 'regulär' ? "400€" : "150€"}}</b>
          </div>
        </div>
      </div>

    </div>
    <div class="spacer"></div>
    <div class="wizard-checkbox-agb">
      <!--<Checkbox :value="agbBool" @click="checkTrue" theme="form">Ja, ich habe die <a class="checkbox-link" :href="window+'/de/agb'" target="_blank"> Allgemeinen Nutzungsbedingungen (ANB) </a> und die <a class="checkbox-link" :href="window+'/de/faq'" target="_blank"> Werkstattordnung </a> gelesen und bin damit einverstanden.</Checkbox>
    -->
      <input type="checkbox" id="agb" name="agb" value="!agbBool" v-model="user.agbBool">
      <label for="agb">Ja, ich habe die <a class="checkbox-link" :href="window+'/de/agb'" target="_blank"> Allgemeinen Nutzungsbedingungen (ANB) </a> und die <a class="checkbox-link" :href="window+'/de/faq'" target="_blank"> Werkstattordnung </a> gelesen und bin damit einverstanden.</label><br>
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
      agbBool: false,
      type: 'regulär',
      periode: 'month'
    }
  },
  created() {
    console.log(this.user);
  },
  methods: {
    checkForm() {

    },
    checkTrue(e) {

    },
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
  .option b {
    padding-left: 25px;
  }
  /*.info {
    position: absolute;
    right: 0;
    margin-right: 23%;
  }*/
}

</style>
