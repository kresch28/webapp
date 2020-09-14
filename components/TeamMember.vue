<template>
  <div v-editable="blok" class="member-page" @touchstart="touch">
    <a :href="back" display="none" ref="hidden" class="breadcrumb"><img class="icon-breadcrumb" src="~/assets/img/icons/arrow-left-solid.svg" alt=""/>
      <b>{{firstName}} {{lastName}} - {{blok.title}}</b></a>
    <div class="header">
      <div class="image">
        <img class="picture" v-if="blok.image" :src="$resizeImage(blok.image, '700x700')" :alt="blok.name + ', ' + blok.title"/>
        <img class="picture image-alt" v-if="blok.image_alt" :src="$resizeImage(blok.image_alt, '700x700')" :alt="blok.name + ', ' + blok.title"/>
      </div>
      <div class="info">
        <div class="short-info">
          <div class="name-contact">
            <div class="name">{{firstName}} <br> {{lastName}}</div>
            <div class="contact-options">
              <a class="option email" v-if="blok.email" :href="'mailto:'+blok.email">
                <img class="icon" src="~/assets/img/icons/envelope.svg" alt=""/>
                <!--<div class="text">{{blok.email}}</div>-->
              </a>
              <a v-if="blok.fb_src" class="option-links" :href="blok.fb_src.url">
                <img class="icon" :src="`/icons/fb.png`">
              </a>
              <a v-if="blok.linkedin_src" class="option-links linkedin" :href="blok.linkedin_src">
                <img class="icon" src="~/assets/img/icons//linkedin.svg">
              </a>
              <a v-if="blok.twitter_src" class="option-links twitter" :href="blok.twitter_src.url">
                <img class="icon" src="~/assets/img/icons/twitter.svg">
              </a>
            </div>
          </div>
          <div class="title">{{blok.title}}</div>
        </div>
        <div class="short-description">
          {{blok.short_description}}
        </div>
      </div>
    </div>
    <div class="short-description-mobile">
      {{blok.short_description}}
    </div>
    <div class="body">
      <div class="future-slogan">
        <div class="first">Die Zukunft</div>
        <div class="second">gehört {{blok.future}}<span v-if="!blok.future">uns allen</span></div>
      </div>
      <div class="description">
        <markdown :value="blok.description"></markdown>
      </div>
    </div>
    <div class="hotspots" v-if="blok.hotspots">
      <div class="hotspots-header">
        <div class="hotspots-title">Meine <br> Hotspots</div>
        <div class="hotspots-link">
          <img src="~/assets/img/arrow-right.svg" class="link-arrow">
          <div>Alle Hotspots</div>
        </div>
      </div>
      <!--<div class="hotspots-items">
        <div v-for="h in blok.hotspots">
          <hotspot :blok="h"></hotspot>
        </div>
      </div>-->
      <div v-editable="blok" class="hotspot-slideshow">
        <div class="hotspot-swiper-container" v-swiper:swiper="swiperOption">
          <div class="hotspot-swiper-wrapper">
            <!--<div v-for="h in blok.hotspots" class="swiper-slide" :style="{ 'background-image': 'url(' + $resizeImage(h.image, '700x0') + ')' }">-->
            <div v-for="h in blok.hotspots" class="hotspot-swiper-slide">
              <div class="hotspot-content" :style="{ 'background-image': 'url(' + $resizeImage(h.image, '700x0') + ')' }">
              <!--<img class="picture" v-if="h.image" :src="$resizeImage(h.image, '700x700')"/>-->
              <span class="title"><b>{{h.title}}</b></span>
              <span class="description">{{h.description}}</span>
              </div>
            </div>
          </div>
          <div v-if="count > 3" class="swiper-button-next"></div>
          <div v-if="count > 3" class="swiper-button-prev"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['blok'],
  created() {
    console.log(this.$route.path.split('/'))
    },
  computed: {
    back() {
      return '/' + this.$route.path.split('/')[1] + '/' + this.$route.path.split('/')[2]
    },
    count() {
      return this.blok.hotspots.length
    },
    firstName() {
      let name = this.blok.name.split(' ');
      return name[0];
    },
    lastName() {
      let name = this.blok.name.split(' ');
      let last = '';
      for (let i = 1; i < name.length; i++) {
        last = last + ' ' + name[i];
      }
      return last;
    },
    swiperOption() {
      return {
        slidesPerView: this.num,
        spaceBetween: this.spaceBetween,
        autoplay: {
          delay: 5000,
          disableOnInteraction: true
        },
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev'
        }
      }
    },
    spaceBetween() {
      if (process.client && window && window.innerWidth) {
        if (window.innerWidth < 786) {
          return 0;
        }
      }
      return 30;
    },
    num() {
      if (process.client && window && window.innerWidth) {
        if (window.innerWidth < 786) {
          return 1;
        }
      }
      return 3;
    },
  },
  methods: {
    touch(e) {
      if (e.target.localName !== 'img') {
        this.$refs.hidden.focus();
      }
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/scss/styles.scss';

.member-page {
  @include margin-page-wide();
  min-height: 150px;
  .breadcrumb {
    color: #000000;
    font-size: smaller;
    .icon-breadcrumb {
      width: 12px;
      margin-right: 15px;
      margin-top: 10px;
    }
  }
  .header {
    display: flex;
    margin-top: 20px;
    .image {
      position: relative;
      flex-grow: 1;
      width: 46%;
      margin-right: 2%;
      text-align: right;
      &:hover {
        .image-alt {
          display: inline;
        }
      }
      .image-alt {
        display: none;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 99;
      }
      .picture {
        width: 100%;
        max-width: 100%;
        max-height: 100%;
      }
    }
    .info {
      display: flex;
      flex-direction: column;
      flex-grow: 1;
      width: 50%;
      margin-left: 2%;
      .short-info {
        display: flex;
        flex-grow: 1;
        flex-direction: column;
        justify-content: flex-end;
        .name-contact {
          align-items: flex-end;
          padding-bottom: 1rem;
          border-bottom: .4rem solid black;
          margin-top: 1rem;
          margin-bottom: 1rem;
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          .name {
            font-family: $font-secondary;
            font-size: 4rem;
            text-transform: uppercase;
            line-height: 1.2;
          }
          .contact-options {
            font-size: .9rem;
            display: flex;
            flex-direction: row;
            justify-content: flex-end;
            .option {
              display: flex;
              flex-direction: row-reverse;
              align-items: center;
              padding: .4em 0;
              color: inherit;
              text-decoration: none;
              .icon {
                width: 18px;
                margin-left: .5em;
              }
              .text {
              }
            }
            .option-links{
              display: flex;
              flex-direction: row-reverse;
              align-items: center;
              padding: .4em 0;
              color: inherit;
              text-decoration: none;
              margin-right: 5px;
              .icon {
                width: 10px;
                margin-left: .5em;
              }
              .text {
              }
            }

            .option-links.twitter{
              .icon {
                width: 18px;
                margin-left: .5em;
              }
            }

            .option-links.linkedin{
              .icon {
                width: 18px;
                margin-left: .5em;
              }
            }
          }
        }
        .title {
          font-family: $font-mono;
          font-size: 1rem;
          margin-bottom: 2em;
        }
      }
      .short-description {
        line-height: 1.5;
        @include media-breakpoint-up(xl) {
          width: 80%;
        }
        font-size: 1rem;
      }
    }
  }
  .short-description-mobile {
    display: none;
  }
  .body {
    padding: 100px 0;
    .future-slogan {
      transform: rotate(-5deg);
      font-size: 2rem;
      @include media-breakpoint-down(sm) {
        font-size: 1.4rem;
      }
      width: 40%;
      min-width: 15em;
      margin-left: 12%;
      margin-bottom: 9%;
      .first {
        font-weight: bold;
        text-transform: uppercase;
        margin-bottom: .2em;
      }
      .second {
        font-family: $font-secondary;
      }
    }
    .description {
      margin: 0 20% 0 30%;
    }
  }

  @include media-breakpoint-down(lg) {
    .header {
      .info {
        .short-info {
          justify-content: flex-start;
          .name-contact {
            flex-direction: column-reverse;
            .name {
              align-self: flex-start;
              font-size: 2.5em;
            }
          }
        }
        .short-description {
          display: none;
        }
      }
    }
    .short-description-mobile {
      display: flex;
      margin: 1em 0;
      line-height: 1.4;
      font-size: 1rem;
    }
  }

  @include media-breakpoint-down(sm) {
    .header {
      flex-direction: column;
      .info {
        width: 100%;
        margin: 0;
        .short-info {
          .name-contact {
            .name {
            }
          }
        }
        .short-description {
        }
      }
      .image {
        width: 400px;
        max-width: 100%;
      }
    }
    .short-description-mobile {
    }
    .body {
      .future-slogan {
        margin-left: 2%;
        margin-bottom: 20%;
      }
      .description {
        margin: 0;
      }
    }
  }

  @include media-breakpoint-down(xs) {
    .header {
      .info {
        .short-info {
          .name-contact {
            .name {
              font-size: 2em;
            }
          }
        }
      }
    }
  }

  .hotspot-slideshow {
    margin-left: auto;
    margin-right: auto;
    width: 90%;
    .hotspot-swiper-container {
      width: 100%;
      height: 100%;

      .hotspot-swiper-wrapper{
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: space-evenly;
        .hotspot-swiper-slide {
          height: 250px;
          width: 33%;

          .hotspot-content {
            background-size: cover;
            height: 100%;
            width: 100%;
            display: flex;
            flex-direction: column;

            .picture {
                  width: 70%;
              height: 50%;
              margin-left: auto;
              margin-right: auto;
            }
            .title {
              width: 70%;
              margin-right: auto;
              margin-top: 280px;
              font-family: $font-secondary;
            }
            .description {
              width: 70%;
              margin-right: auto;
              margin-top: 15px;
              font-family: $font-secondary;
            }
          }
        }

        padding-bottom: 60px;
      }
    }
    .swiper-button-prev,
    .swiper-button-next {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background-color: $color-yellow;
      background-size: 12px;
    }
  }

}
.hotspots-header{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 70%;
  margin-left: 160px;
  margin-bottom: 60px;
  .hotspots-title {
    font-size: 3rem;
    font-weight: bold;
    text-transform: uppercase;
    margin-bottom: .2em;
  }
  .hotspots-link {
    display: flex;
    flex-direction: row;
    justify-content: center;
    margin-top: 60px;
    .link-arrow {
      width: 10%;
      height: 20%;
      margin-right: 15px;
    }
  }
}

  .hotspots-items {
    display: flex;
    justify-content: center;
      margin-left: auto;
      margin-right: auto;
    width: 70%;
  }



</style>
