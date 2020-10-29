<template>
  <div class="section">
    <h2>Kontaktdaten</h2>
    <!--<form class="form" @submit.prevent="updateUser">-->
      <!--
      <div class="form-item">
        <span class="label">Vorname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.firstName" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Nachname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.lastName" name="" id=""/>
      </div>
      -->
      <div class="form-item">
        <div class="info-block">
          <span class="label">Geburtsdatum</span>
          <input class="input-text" type="date" v-model="user.profile.birthdate" name="" id=""/>
        </div>
      </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label">Telefon</span>
          <input class="input-text" type="text" v-model="user.profile.phone" name="" id="" placeholder="+43"/>
        </div>
      </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label">Adresse</span>
          <input class="input-text" type="text" v-model="user.profile.address" name="" id=""/>
        </div>
      </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label"></span>
          <input class="input-text" type="text" v-model="user.profile.address2" name="" id=""/>
        </div>
      </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label">PLZ</span>
          <input class="input-text" type="text" v-model="user.profile.zip" name="" id=""/>
        </div>
      </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label">Stadt</span>
          <input class="input-text" type="text" v-model="user.profile.city" name="" id=""/>
        </div>
      </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label">Firma</span>
          <input class="input-text" type="text" v-model="user.profile.company" name="" id=""/>
        </div>
      </div>
    <!--</form>-->
    <div class="wizard-checkbox">
      <p>Wir gehen verantwortungsvoll mit deinen Daten um.</p>
      <label>
        <!--<Checkbox :value="dsBool" theme="form">Ja, ich habe die <a class="checkbox-link" :href="window+'/de/datenschutzerklaerung'" target="_blank">Datenschutzerklärung</a> gelesen und bin damit einverstanden.</Checkbox>
        -->
        <input type="checkbox" id="ds" name="ds" value="!dsBool" v-model="user.dsBool">
        <label for="ds">Ja, ich habe die <a class="checkbox-link" :href="window+'/de/datenschutzerklaerung'" target="_blank">Datenschutzerklärung</a> gelesen und bin damit einverstanden.</label><br>

      </label>
    </div>
  </div>
</template>

<script>
import Checkbox from "~/components/Checkbox.vue";

export default {
  middleware: 'authenticated',
  components: {
    Checkbox
  },
  data () {
    return {
      loading: false,
      dsBool: false
    }
  },
  created() {
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
  .info-block {
    display: flex;
    justify-content: space-between;
    width: 50%;
  }
  .checkbox-link {
    padding-left: 5px;
    padding-right: 5px;
  }
  .label {
    padding-right: 10px;
  }
  .form-item {
    margin: 15px;
  }
  .wizard-checkbox {
    margin-top: 40px;
  }
</style>
