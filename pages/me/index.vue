<template>
  <div v-if="user.packages.length !== 0" class="section">
    <h2>Kontaktdaten</h2>
    <form class="form" @submit.prevent="updateUser">
      <div class="form-item">
        <span class="label">Vorname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.firstName" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Nachname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.lastName" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Adresse</span>
        <input class="input-text" type="text" v-model="user.profile.address" name="" id=""/>
      </div>
      <div class="form-item">
        <span></span>
        <input class="input-text" type="text" v-model="user.profile.address2" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">PLZ</span>
        <input class="input-text" type="text" v-model="user.profile.zip" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Stadt</span>
        <input class="input-text" type="text" v-model="user.profile.city" name="" id=""/>
      </div>
      <div class="button-row">
        <div v-if="loading">
          Saving…
        </div>
        <button v-else type="submit" class="input-button-primary" @click="updateUser">Speichern</button>
      </div>
    </form>
    <h2>Deine Badges</h2>
    <p>Hier findest du deine Auszeichnungen für bestandene Kurse und MSUs und du bekommst Auszeichnungen für Maschinenstunden verliehen:</p>
    <!--<div class="badge-container">
  </div>-->
    <div class="badge-info">
      <div class="info-row">
        <span class="badge-header">Kurse und MSUs</span>
      </div>
      <div class="info-row">
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Lasercutter.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>Lasercut-Hero für 10 Stunden Lasercutten</p>
            </div>
          </div>
        </div>
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Workshop.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>ASU erfolgreich abgeschlossen</p>
            </div>
          </div>
        </div>
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Lasercutter.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>Lasercut-Hero für 10 Stunden Lasercutten</p>
            </div>
          </div>
        </div>
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Workshop.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>ASU erfolgreich abgeschlossen</p>
            </div>
          </div>
        </div>
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Workshop.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>ASU erfolgreich abgeschlossen</p>
            </div>
          </div>
        </div>
      </div>
      <div class="info-row">
        <span class="badge-header">Maschinenstunde</span>
      </div>
      <div class="info-row">
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Lasercutter.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>Lasercut-Hero für 10 Stunden Lasercutten</p>
            </div>
          </div>
        </div>
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Lasercutter.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>Lasercut-Hero für 10 Stunden Lasercutten</p>
            </div>
          </div>
        </div>
        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <img src="~/assets/img/Garmin_Connect_Badges_Workshop.jpg" alt="Avatar" style="width:100px;height:100px;">
            </div>
            <div class="flip-card-back">
              <p>ASU erfolgreich abgeschlossen</p>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
  <div v-else class="section">
    <h2>Mitgliedschaft</h2>
    <a :href="window+'/de/mitgliedschaften'" target="_blank">Hier bekommst du alle Infos zur Mitgliedschaft</a>
    <component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content">
      <markdown class="info" :value="story.content.body[0].info"></markdown>
      <div class="membership-details">
        <div class="payment-options">
          <div class="payment-options-title">Zahlungsintervall:</div>
          <div class="pricetabs">
            <div @click="setPriceView('monthly')" class="pricetab" :class="(priceView == 'monthly' ? 'active' : '')">
              monatlich
            </div>
            <div @click="setPriceView('annually')" class="pricetab" :class="(priceView == 'annually' ? 'active' : '')">
              jährlich
            </div>
          </div>
        </div>
        <div class="membership-plans">
          <component v-for="blok in story.content.body[0].columns" :blok="blok" :priceView="priceView" :is="blok.component" v-bind:key="blok.id"></component>
        </div>
        <div class="register-button">
          <!--<div class="register-button" v-if="!hasUser">
          <button @click="register">Jetzt Mitglied werden</button>-->
          <div class="link-with-arrow">
            <div class="link-text">
              <Nuxt-Link to="/wizard/onboarding/" class="link">Mitglied werden</Nuxt-Link>
            </div>
          </div>
        </div>
        <!--<div v-if="blok.plans_text" class="plans-text">
          <markdown :value="blok.plans_text"></markdown>
        </div>-->
      </div>
    </component>
  </div>
</template>

<script>
  import storyblokLivePreview from '@/mixins/storyblokLivePreview'
  export default {
    props: ['blok'],
    middleware: 'authenticated',
    mixins: [storyblokLivePreview],
    data () {
      return {
        messages: {},
        hover: false,
        loading: false,
        title: 'Grand Garage Profile',
        priceView: 'monthly',
        inUser: '',
        newInUser: {
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
        }
      }
    },
    created() {
      this.$store.dispatch("getInvoiceContact", this.profileData).then((data) => {
        this.inUser =  data.data;
      }).catch((err)=> {
        console.log(err);
      });
    },
    head() {
      return {
        title: this.story.content.metadata.title,
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.story.content.metadata.description,
          }
        ]
      }
    },
    methods: {
      change : function(key, message) {
        this.$set(this.messages, key, message)
      },
      reset() {
        this.messages = {};
      },
      register() {
        //console.log(this.priceView);
      },
      setPriceView(v) {
        this.priceView = v;
        //console.log(this.priceView);
      },
      updateUser(event) {
        this.loading = true;
        this.newInUser.profile_type = this.inUser.profile_type;
        this.newInUser.profile_period = this.inUser.profile_period;
        this.newInUser.firstname = this.user.profile.firstName;
        this.newInUser.lastname = this.user.profile.lastName;
        this.newInUser.birthdate = this.inUser.birthdate;
        this.newInUser.phone = this.inUser.phone;
        this.newInUser.street = this.user.profile.address;
        this.newInUser.street_additional = this.user.profile.address2;
        this.newInUser.zip = this.user.profile.zip;
        this.newInUser.city = this.user.profile.city;
        this.newInUser.country = 'at';
        this.newInUser.iban = this.inUser.iban;
        this.newInUser.bic = this.inUser.bank;
        this.$store.dispatch("updateInvoiceContact", this.newInUser).then((data) => {
        }).catch((err)=> {
          console.log(err);
        });

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
      invoiceUser(){
        return this.inUser;
      },
      window(){
        return window.location.origin
      },
    },
    asyncData (context) {
      let path = '/members/index';
      return context.store.dispatch('loadPage', path).catch((e) => {
        context.error({ statusCode: e.response.status, message: e.response.statusText })
      });
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/assets/scss/styles.scss';

  .info{
    margin-top: 40px;
  }

  .membership-details {
    flex-basis: 50%;
    justify-content: center;
    .payment-options {
      display: flex;
      @include media-breakpoint-down(xs) {
        flex-direction: column;
        .payment-options-title {
          margin-bottom: .4em;
        }
      }
      margin: 4vh 0 3vh;
      align-items: center;
      justify-content: center;
      font-size: .8em;
      text-transform: uppercase;

      .payment-options-title {
        font-weight: bold;
        margin-right: .7em;
      }
      .pricetabs {
        display: inline-flex;
        .pricetab {
          cursor: pointer;
          padding: .4em 1em;
          &.active {
            background-color: $color-orange;
            border-radius: 1em;
            color: #fff;
          }
        }
      }
    }
    .membership-plans {
      display: flex;
      flex-direction: row;
      justify-content: center;
    }
    .register-button {
      text-align: center;
      button {
        outline: none;
        cursor: pointer;
        font-size: 1.2em;
        font-weight: bold;
        color: #FFF;
        border: none;
        padding: 15px;
        background-color: $color-orange;
        margin: 1.5em 0;
        transition: background-color .3s linear;
        &:hover {
          background-color: saturate(darken($color-orange, 5%), 100%);
        }
      }
      .link {
        background-color: #ff6f00;
        color: #FFFFFF;
        margin-top: 20px;
        padding: 20px;
      }
    }

  }
  .badge-container {
    display: flex;
    margin-top: 20px;
  }
  .badge-item {
    width: 60%;/*
    display: flex;
    justify-content: space-between;*/
    margin-bottom: 10px;
    @include media-breakpoint-up(xl) {
      width: 40%;
    }
    @include media-breakpoint-down(sm) {
      width: 100%;
    }
    .icon {
      width: 100%;
      /*@include media-breakpoint-up(xl) {
        width: 12%;
      }*/
    }
  }

  /*.badge-info {
    padding: 20px;
    margin: 10px;
    position: relative;
    left: 100px;
    top: -70px;
    @include media-breakpoint-up(xl) {
      top: -80px;
    }
  }*/
  .flip-card {
    background-color: transparent;
    width: 250px;
    height: 150px;
    perspective: 1000px; /* Remove this if you don't want the 3D effect */
    flex: 1 0 21%;
    @include media-breakpoint-down(xs) {
      width: 50%;
    }
  }

  /* This container is needed to position the front and back side */
  .flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;
  }

  /* Do an horizontal flip when you move the mouse over the flip box container */
  .flip-card:hover .flip-card-inner {
    transform: rotateY(180deg);
  }

  /* Position the front and back side */
  .flip-card-front, .flip-card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden; /* Safari */
    backface-visibility: hidden;
  }

  /* Style the front side (fallback if image is missing) */
  .flip-card-front {
    color: black;
  }

  /* Style the back side */
  .flip-card-back {
    background-color: #ffffff;
    color: black;
    transform: rotateY(180deg);
  }

  .badge-info {
    background-color: #FFF;
    border: 1px solid $color-orange;
    border-radius: 5px;
    padding: 40px 30px;
    position: relative;
    @include media-breakpoint-up(sm) {
      float: left;
    }
    @include media-breakpoint-down(sm) {
      margin: 10px 0px;
      width: 100%;
    }
    .info-row {
      font-family: $font-mono;
      font-size: 0.9rem;
      font-weight: bold;
      margin-bottom: 20px;
      display: flex;
      line-height: 1.6;
      justify-content: space-evenly;
      flex-wrap: wrap;
      .info-block {
        flex-direction: row;
        display: flex;
        .log-info {
          margin-top: 10px;
          width: 75%;
          .heading{
            font-weight: 500;
          }
        }
        .col {
          padding: 8px;
        }
      }
      .info-block.left{
        flex: 2;
      }
      .info-block.right {
        flex: 1;
      }

    }
    .more {
      background-color: #ff6f00;
      border: 1px solid #ff8c33;
      color: #FFF;
      cursor: pointer;
      left: 45%;
      line-height: 1;
      outline: none;
      padding: 7px 12px 8px;
      position: absolute;
      right: 50%;
      margin-top: 5px;
      @include media-breakpoint-down(sm) {
        left: 40%;
      }
    }
  }
</style>
