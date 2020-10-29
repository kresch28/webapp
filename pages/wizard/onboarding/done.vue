<template>
  <div class="section">
    <!--<h2>FIN</h2>

    <form class="form" name="signup" @submit.prevent="handleSubmit" data-netlify="true" netlify-honeypot="bot-field">
      <label class="hidden"><input name="bot-field" /></label>
      <div data-netlify-recaptcha="true"></div>
      <label class="form-item">
        <span class="label">Test</span>
        <div class="body">
          <input class="input-text" type="name" name="name" v-model="form.name" placeholder="Dein Name">
        </div>
      </label>
      <div class="button-row">
        <button type="submit" class="input-button-primary">Abschicken</button>
      </div>
    </form>-->
   <h2>Übersicht:</h2>
    <div class="col">
      <div class="plan">
        <h2 class="title">BASIC GARAGE {{this.user.periode == 'month' ? 'monatlich' : 'jährlich'}}</h2>
        <transition name="changeprice">
          <div class="pricewrapper" v-if="this.user.periode == 'month'">
            <div class="price" v-if="this.user.type == 'ermäßigt'">
              <h4 class="title">Ermäßigt</h4>
              <div class="pricetag">
                <div class="price-value">15,- €</div>
              </div>
            </div>
            <div class="price" v-if="this.user.type == 'regulär'">
              <h4 class="title">Regulär</h4>
              <div class="pricetag">
                <div class="price-value">40,- €</div>
              </div>
            </div>
          </div>
          <div class="pricewrapper" v-if="this.user.periode == 'year'">
            <div class="price" v-if="this.user.type == 'ermäßigt'">
              <h4 class="title">Ermäßigt</h4>
              <div class="pricetag">
                <div class="price-value">150,- €</div>
              </div>
            </div>
            <div class="price" v-if="this.user.type == 'regulär'">
              <h4 class="title">Regulär</h4>
              <div class="pricetag">
                <div class="price-value">400,- €</div>
              </div>
            </div>
          </div>
        </transition>
        <div class="price">
          <h4 class="title">Zahlungsmethode: SEPA</h4>
          <div class="pricetag">
            <div class="price-value_">IBAN: {{this.user.payment.iban}}</div>
            <div class="price-value_">BIC: {{this.user.payment.bank}}</div>
          </div>
        </div>
        <!--<ul class="feature-list">
          <li class="feature">Geburtsdatum: {{this.user.profile.birthdate}}</li>
        </ul>-->
        <div class="col">
          <div class="plan">
            <ul class="item-list">
              <li class="feature">{{this.user.profile.firstName}} {{this.user.profile.lastName}}</li>
              <li class="feature" v-if="this.user.profile.birthdate != null">Geburtsdatum: {{this.user.profile.birthdate}}</li>
              <li class="feature" v-if="this.user.profile.phone != null">Telefon: {{this.user.profile.phone}}</li>
              <li class="feature" v-if="this.user.profile.address != null">Adresse: {{this.user.profile.address}}</li>
              <li class="feature" v-if="this.user.profile.address2 != null">{{this.user.profile.address2}}</li>
              <li class="feature" v-if="this.user.profile.zip != null">ZIP: {{this.user.profile.zip}}</li>
              <li class="feature" v-if="this.user.profile.city != null">Ort: {{this.user.profile.city}}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  middleware: 'authenticated',
  data () {
    return {
      loading: false,
      type: '',
      periode: '',
      birthdate: '',
    }
  },
  created() {
    console.log(this.user)
  },
  methods: {
    encode (data) {
      return Object.keys(data)
        .map(
          key => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`
        )
        .join("&");
    },
    handleSubmit () {
      this.loading = true;
      const axiosConfig = {
        header: { "Content-Type": "application/x-www-form-urlencoded" }
      };
      axios.post(
        "/",
        this.encode({
          "form-name": "signup",
          ...this.form
        }),
        axiosConfig
      ).then(() => {
        this.loading = false;
        this.form.msg = '';
        this.sent = true;
      }).catch(() => {
        this.loading = false;
      });
    }
  },
  computed: {
    form() {
      return {
        ...this.user.profile,
      }
    },
    user() {
      return this.$store.state.user;
    },
  }
}
</script>

<style lang="scss" scoped>
  @import '@/assets/scss/styles.scss';
.form {
  .hidden {
    display: none;
  }
}

  .col {
    display: flex;
    justify-content: space-between;
    .plan {
      max-width: 500px;
      padding-left: 3vw;
      padding-right: 3vw;
      padding-top: 40px;
      background-color: #FFF;
      display: flex;
      flex-direction: column;
      flex-grow: 1;
      margin-bottom: 2vh;
      h2.title {
        font-weight: normal;
        margin: 0;
        font-family: $font-secondary;
        color: $color-blue-alt;
        min-height: 2em;
      }
      ul.item-list {
        margin: 10px 0;
        list-style-type: none;
        padding: 10px 0;
        flex-grow: 1;
        li.feature {
          padding: 10px 0;
          font-size: 0.8rem;
          font-family: $font-mono;
          border-bottom: 1px solid #aaa;
          line-height: 1.5em;
          &:last-child {
            border: none;
          }
        }
      }
    }
  }
  .price-value_ {
    padding-top: 10px;
  }
</style>
