<template>
  <div>
    <div class="machine-list-item">
      <div class="image">
        <machine-status class="status" :id="fabmanId"></machine-status>
        <img :src="$resizeImage(content.image, '450x450')" alt=""/>
      </div>
      <div class="body">
        <div class="title">
          {{content.title}}
        </div>
        <div class="tags">
          <span v-for="(tag, index) in tags">
            {{tag}}<span v-if="index+1 < tags.length">, </span>
          </span>
        </div>
        <div class="teaser">
          <markdown :value="content.teaser"></markdown>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MachineStatus from '@/components/MachineStatus';

export default {
  props: ['blok'],
  components: {
    MachineStatus
  },
  computed: {
    fabmanId() {
      return this.blok.content.machine_status_items[0].fabmanId;
    },
    content() {
      return this.blok.content;
    },
    tags() {
      return this.blok.tag_list;
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/styles.scss';
a {
  text-decoration: none;
  color: #000;
  &:active,
  &:focus {
    color: #000;
  }
}

.machine-list-item {
  display: flex;
  flex-direction: column;
  margin-bottom: 25px;
  .image {
    flex: 1;
    padding-top: 1.4em;
    img {
      display: block;
      width: 100%;
    }
  }
  .body {
    flex: 2;
    padding: 20px 0;
    .title {
      font-family: $font-secondary;
      font-size: 1.2rem;
      margin-bottom: .4em;
    }
    .tags {
      font-size: 0.9rem;
      color: $color-blue;
      text-transform: uppercase;
      margin-bottom: .8rem;
      letter-spacing: 0.05em;
      font-weight: 400;
    }
    .subtitle {
      margin:  0 0 1em 0;
    }
    .teaser {
      font-family: $font-mono;
      line-height: 2.2;
      font-size: 0.9rem;
    }
    .cta {
      text-transform: uppercase;
      font-weight: bold;
      font-size: 0.9rem;
      font-family: $font-secondary;
      letter-spacing: 0.05rem;
      padding: 25px 0;
      a {
        color: $color-blue;
        text-decoration: none;
      }
    }
  }
}
</style>
