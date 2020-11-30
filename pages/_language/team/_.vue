<template>
  <section class="util__container">
    <component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component"></component>
  </section>
</template>

<script>
import storyblokLivePreview from '@/mixins/storyblokLivePreview'

export default {
  data () {
    return {
      story: null
    }
  },
head() {
    return {
      title: this.story.content.metadata.title,
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          hid: 'description',
          name: 'description',
          content: this.story.content.metadata.description,
        }
      ]
    }
  },
  created() {
    console.log(this.story) },
  mixins: [storyblokLivePreview],
  asyncData (context) {
    return context.store.dispatch('loadFullPage', context.route.fullPath).catch((e) => {
      context.error({ statusCode: e.response.status, message: e.response.statusText })
    });
  }
}
</script>
