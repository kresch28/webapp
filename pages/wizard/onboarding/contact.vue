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
        <span class="label">Foto für deinen Mietgliedsausweis</span>
        <label for="picture" class="picture-file-upload">
          Datei hochladen
          <input type="file" v-on:change="getPicture" ref="picture" id="picture">
        </label>
        </div>
      <img class="preview" :src="imageData">
      </div>
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
        <span class="label">Land</span>
        <input class="input-text" type="text" v-model="user.profile.state" name="" id=""/>
      </div>
    </div>
      <div class="form-item">
        <div class="info-block">
          <span class="label">Firma</span>
          <input class="input-text" type="text" v-model="user.profile.company" name="" id=""/>
        </div>
      </div>
    <!--</form>-->

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
      dsBool: false,
      picture: null,
      imageEvent: null,
      imageData: null
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
    },
    getPicture(e) {
      console.log(e.target.value);
      this.picture = this.$refs.picture.files[0];
      var input = e.target;
      if (input.files && input.files[0]) {
        var reader = new FileReader();
        reader.onload = (event) => {
          this.imageData = event.target.result;
        }
        reader.readAsDataURL(input.files[0]);
      }
      console.log(this.picture);
      this.user.picture = this.picture;

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
    @include media-breakpoint-up(sm) {
      width: 80%;
    }
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
  input[type="file"] {
    display: none;
  }
  .picture-file-upload {
    cursor: pointer;
    background-color: #ff6f00;
    color: #FFF;
    min-width: 40%;
    border: 1px solid #ff8c33;
    padding: 7px 12px 8px;
    line-height: 1;
    outline: none;
  }
</style>
