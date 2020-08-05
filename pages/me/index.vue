<template>
  <div v-if="user.packages.length == 0" class="section">
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
          <component v-for="blok in story.content.body[0].columns" :blok="blok" :priceView="priceView" :is="blok.component"></component>
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
        loading: false,
        priceView: 'monthly'
      }
    },
    created() {
      console.log(this.story);
    },
    methods: {
      register() {
        console.log(this.priceView);
      },
      setPriceView(v) {
        this.priceView = v;
        console.log(this.priceView);
      },
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
</style>
