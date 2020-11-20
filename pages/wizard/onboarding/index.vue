<template>
  <div class="section onboarding-wizard">
    <h2>Du willst also eine Mitgliedschaft abschließen?</h2>
    <p>für <b>ermäßigt</b> bist du berechtigt bei Vorlage folgender Ausweise: 4you card, SchülerInnen, Studierende, Lehrlinge bis 28 Jahre, Behindertenpass</p>
    <form id="index" @submit="checkForm" method="post">
    <div class="options">
      <div class="option" v-bind:class="{missing: user.errors.type === false}">
        <input class="radio" type="radio" v-model="user.type" id="regulär" name="type" value="regulär" @click="checkTrue">
        <label for="regulär">
          <div class="answer-text">regulär</div>
        </label>
        </div>
      <div class="option" v-bind:class="{missing: user.errors.type === false}">
        <input class="radio"  type="radio" v-model="user.type" id="ermäßigt" name="type" value="ermäßigt" @click="checkTrue">
        <label for="ermäßigt">
          <div class="answer-text">ermäßigt</div>
        </label>
        </div>
      <div class="option" v-bind:class="{missing: user.errors.type === false}">
        <input class="radio" type="radio" v-model="user.type" id="free" name="type" value="free" @click="checkTrue">
        <label for="free">
          <div class="answer-text">free</div>
        </label>
        </div>
    </div>
    <div>
      <div v-if="user.type != 'free'">
        <div class="info">
          <p><b>Tipp:</b> Bei jährlicher Zahlung bekommst du 2 Monate geschenkt.</p>
        </div>
        <div class="options">
          <div class="option" v-bind:class="{missing: user.errors.periode === false}">
            <input class="radio" type="radio"  v-model="user.periode" id="month" name="periode" value="month" @click="checkTrue">
            <label for="month">
              <div class="answer-text">monatlich {{ user.type == 'regulär' ? "40€" : "15€"}}</div>
            </label>
          </div>
          <div class="option" v-bind:class="{missing: user.errors.periode === false}">
            <input class="radio" type="radio"  v-model="user.periode" id="year" name="periode" value="year" @click="checkTrue">
            <label for="year">
              <div class="answer-text">jährlich {{ user.type == 'regulär' ? "400€" : "150€"}}</div>
            </label>
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
      empty: true,
      type: 'regulär',
      periode: 'month'
    }
  },
  created() {
  },
  methods: {
    checkForm() {

    },
    checkTrue() {
      let counter = 1;
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
  .options {
    padding: 20px 0;
    display: flex;
    justify-content: space-around;
    margin: 0 -10px;

    .option {
      margin: 10px;
      cursor: pointer;
      padding: 25px;
      background-color: #FFF;
      display: flex;
      flex-direction: row;
      width: 50%;
      label {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-left: auto;
        margin-right: auto;
        .answer-text {
          padding: 0 70px;
        }
      }
      &:hover {
        border: 2px solid $color-orange;
      }
    }
  }
  /*.info {
    position: absolute;
    right: 0;
    margin-right: 23%;
  }*/
}

</style>
