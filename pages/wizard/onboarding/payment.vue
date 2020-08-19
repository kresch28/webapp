<template>
  <div class="section onboarding-wizard">
    <h2>Zahlung</h2>
    <div v-if="this.user.type === 'ermäßigt'">
      <p>Bestätige deine Berechtigung, dass du eine ermäßigte Mitgliedschaft bekommen kannst:</p>
      <p>Lade eines der folgenden Dokumente oder eines der folgenden Ausweise hoch: 4you card, SchülerInnen, Studierende, Lehrlinge bis 28 Jahre, Behindertenpass</p>
      <label for="file" class="custom-file-upload">
        Datei hochladen
      <input type="file" v-on:change="getFile" ref="file" id="file">
      </label>
    </div>
      <!--<div class="options">
      <div class="option">
        <b>monatliche Zahlung {{}}</b>
        <p>40 / Monat</p>
      </div>
      <div class="option">
        <b>jährliche Zahlung</b>
        <p>400 / Jahr</p>
      </div>
    </div>
    <p><b>Tipp:</b> Bei jährlicher Zahlung bekommst du 2 Monate geschenkt.</p>
    -->
    <h2 class="payment-option">Wie möchtest du zahlen?</h2>
    <form class="form wizard">
      <div class="form-item">
        <span class="label">IBAN</span>
        <input class="input-text" v-bind:class="{missing: user.errors.iban === false}" type="text" v-model="user.payment.iban" name="" id=""/>
      </div>
      <p class="alert-payment" v-if="user.ibanError">Bitte gib eine gültige IBAN Nummer an</p>
      <div class="form-item">
        <span class="label">BIC</span>
        <input class="input-text"  v-bind:class="{missing: user.errors.bank === false}" type="text" v-model="user.payment.bank" name="" id=""/>
      </div>
      <p class="alert-payment" v-if="user.bankError">Bitte gib einen gültigen BIC an</p>
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
  data () {
    return {
      sepaBool: false,
      loading: false,
      type: null,
      reduction: null,
      bank: '',
      iban: '',
      file: '',
      ibanError: 'false',
      bankError: 'false',
    }
  },
  created() {
  },
  methods: {
    getFile(e) {
      console.log(e.target.value);
      this.file = this.$refs.file.files[0];
      console.log(this.file);
      this.user.file = this.file;

    }
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
  }
}
</script>

<style lang="scss">
@import '@/assets/scss/styles.scss';

.onboarding-wizard {
  .options {
    padding: 20px 0;
    display: flex;
    justify-content: space-around;
    margin: 0 -10px;
    @include media-breakpoint-down(sm) {
      display: block;
    }
    .option {
      margin: 10px;
      flex: 1;
      cursor: pointer;
      padding: 25px;
      background-color: #FFF;
      // border: 2px solid #FFF;
      &:hover {
        border: 2px solid $color-orange;
      }
      .missing {
        border: 1px solid #ff0000;
      }
    }
  }
  .wizard-checkbox {
    max-width: 500px;
  }
  .form {
    &.wizard {
      margin: 20px 0;
    }
  }
  .payment-option {
    margin-top: 40px;
  }
}

input[type="file"] {
  display: none;
}

.custom-file-upload {
  margin-top: 20px;
  cursor: pointer;
  background-color: #ff6f00;
  color: #FFF;
  min-width: 30%;
  border: 1px solid #ff8c33;
  padding: 7px 12px 8px;
  line-height: 1;
  outline: none;
}

  .alert-payment {
    color: #FF0000;
    text-align: center;
  }
</style>
