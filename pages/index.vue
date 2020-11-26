<template>
  <section class="util__container">
    <component v-if="story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component"></component>
  </section>
</template>

<script>
export default {
  data () {
    return {
      story: { content: {} },
      title: 'Grand Garage Makerspace Tabakfabrik Linz 3D Druck Werkstatt'
    }
  },
  asyncData (context) {
    return context.store.dispatch("loadPage", "/");
  },
  head() {
    return {
      title: this.story.content.metadata.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.story.content.metadata.content,
        }
      ]
    }
  }
}
</script>
