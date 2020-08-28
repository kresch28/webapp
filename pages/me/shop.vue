<template>
  <!--<component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component"></component>
  -->
  <div>
    <div class="products-list-wrapper">
      <div v-if="items && items.length>0" class="products-list">
        <!--<transition-group name="list">-->
          <product-list-item v-for="item in items" :blok="item" :key="item.id" class="list-item"></product-list-item>
        <!--</transition-group>-->
      </div>
    </div>
  </div>
</template>

<script>
import storyblokLivePreview from '@/mixins/storyblokLivePreview'

export default {
  middleware: 'authenticated',
  mixins: [storyblokLivePreview],
  data () {
    return {
      products: [],
      tags: [],
    }
  },
  created() {
    console.log(this.products);
  },
  methods: {
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    items() {
      return this.products;
    }
  },
  async asyncData (context) {
    /*let path = '/members/shop';
    return context.store.dispatch('loadPage', path).catch((e) => {
      context.error({ statusCode: e.response.status, message: e.response.statusText })
    });*/

    let tags = await context.store.dispatch("loadTags");
    let filtersM = {
      filter_query: {
        'component': {
          'in': 'machine'
        }
      }
    };
    let products = await context.store.dispatch("findStatusMachines", filtersM).then((data) => {
      if (data.stories) {
        console.log(data.stories);
        return { products: data.stories };
      }
      return { products: [] };
    });
    return {tags, ...products};
  }
}
</script>

<style lang="scss">
  @import '@/assets/scss/styles.scss';
  .products-list {
    display: flex;
    flex-wrap: wrap;
    margin-left: -20px;
  }

  .list-item {
    @include media-breakpoint-down(md){
      width: 50%;
    }
    @include media-breakpoint-down(sm){
      width: 100%;
    }
  }
</style>
