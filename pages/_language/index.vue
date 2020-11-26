<template>
  <section class="">
    <component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component"></component>
  </section>
</template>

<script>
import storyblokLivePreview from '@/mixins/storyblokLivePreview'

export default {
  data () {
    return {
      story: null,
      title: 'Grand Garage'
    }
  },
  mixins: [storyblokLivePreview],
  asyncData (context) {
    return context.store.dispatch("loadPage", "/").catch((e) => {
      context.error({ statusCode: e.response.status, message: e.response.statusText })
    });
  },
  head() {
    return {
      title: this.title,
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          hid: 'description',
          name: 'description',
          content: 'Grand Garage Makerspace Tabakfabrik Linz 3D Druck Werkstatt'
        }
      ]
    }
  },
}
</script>
