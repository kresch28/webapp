<template>
  <section>
    <div class="team-wrapper">
      <img class="image" :src="$resizeImage(story.content.image, '1600x0')">
      <div class="team">
        <div class="headline">
          Die
          <span class="strike">Maschinen</span>
          <br>Menschen dahinter
        </div>
        <div class="subline">
          <markdown :value="story.content.introduction"></markdown>
        </div>
        <select class="member-filters" id="departments" v-model="filters" v-on:change="filter">
          <option v-for="d in departments" v-bind:value="d">{{d}}</option>
        </select>
        <div v-if="filters == 'GESCHÄFTSFÜHRUNG' || filters == 'Departments filtern'">
          <div class="category">GESCHÄFTSFÜHRUNG</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.category == 'GESCHÄFTSFÜHRUNG'" :story="m"></team-member-preview>
          </div>
        </div>
        <div v-if="filters == 'OFFICE' || filters == 'Departments filtern'">
          <div class="category">OFFICE</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.category == 'OFFICE'" :story="m"></team-member-preview>
          </div>
        </div>
        <div v-if="filters == 'WERKSTATT OG1' || filters == 'Departments filtern'">
          <div class="category">WERKSTATT OG1</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.category == 'WERKSTATT OG1'" :story="m"></team-member-preview>
          </div>
        </div>
        <div v-if="filters == 'WERKSTATT OG2' || filters == 'Departments filtern'">
          <div class="category" >WERKSTATT OG2</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.category == 'WERKSTATT OG2'" :story="m"></team-member-preview>
          </div>
        </div>
        <div  v-if="filters == 'KOLLABORATION' || filters == 'Departments filtern'">
          <div class="category">KOLLABORATION</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.category == 'KOLLABORATION'" :story="m"></team-member-preview>
          </div>
        </div>
        <div  v-if="filters == 'KOMMUNIKATION' || filters == 'Departments filtern'">
          <div class="category">KOMMUNIKATION</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.title.includes('Communications')" :story="m"></team-member-preview>
          </div>
        </div>
        <div v-if="filters == 'IT' || filters == 'Departments filtern'">
          <div class="category">IT</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.title.includes('IT')" :story="m"></team-member-preview>
          </div>
        </div>
        <div  v-if="filters == 'HAUSTECHNIK' || filters == 'Departments filtern'">
          <div class="category">HAUSTECHNIK</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.title == 'Haustechnik'" :story="m"></team-member-preview>
          </div>
        </div>
        <div v-if="filters == 'FRONTDESK' || filters == 'Departments filtern'">
          <div class="category">FRONTDESK</div>
          <div class="member-grid">
            <team-member-preview :key="m.id" v-for="m in members" v-if="m.content.title.includes('Frontdesk')" :story="m"></team-member-preview>
          </div>
        </div>
      </div>
    </div>
    <div
      class="image-footer"
      :style="{ 'background-image': 'url(' + $resizeImage(story.content.footerImage, '1600x0') + ')' }"
    ></div>
    <!--
    <component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component" />
    -->
  </section>
</template>

<script>
import storyblokLivePreview from "@/mixins/storyblokLivePreview";

export default {
  data() {
    return {
      story: null,
      filters: 'Departments filtern',
      all: [],
      departments : {
        'Departments filtern' : 'Departments filtern',
        'GESCHÄFTSFÜHRUNG' : 'GESCHÄFTSFÜHRUNG',
        'OFFICE' : 'OFFICE',
        'WERKSTATT OG1' : 'WERKSTATT OG1',
        'WERKSTATT OG2' : 'WERKSTATT OG2',
        'KOLLABORATION' : 'KOLLABORATION',
        'KOMMUNIKATION' : 'KOMMUNIKATION',
        'IT' : 'IT',
        'HAUSTECHNIK' : 'HAUSTECHNIK',
        'FRONTDESK' : 'FRONTDESK',
      }
    };
  },
  created() {
    console.log(this.members)
    },
  methods: {
    filter(){
      console.log(this.filters);
      for (var i = 0; i < this.members.length; i++) {
        if(this.members[i].content.category == this.filters) {
          this.all.push(this.members[i]);
        }
      }
      console.log(this.all);
    }
  },
  mixins: [storyblokLivePreview],
  async asyncData(context) {
    let team = await context.store
      .dispatch("loadTeam")
      .catch(e => {
        context.error({
          statusCode: e.response.status,
          message: e.response.statusText
        });
      })
      .then(res => {
        return { members: res.stories };
      });
    let page = await context.store.dispatch("loadPage", "/team").catch(e => {
      context.error({
        statusCode: e.response.status,
        message: e.response.statusText
      });
    });
    return { ...team, ...page };
  }
};
</script>

<style lang="scss">
@import "@/assets/scss/styles.scss";
.team-wrapper {
  padding-left: 15%;
  padding-top: 15%;
  position: relative;

  @media (max-width: $mobile-small) {
    padding-left: 0;
    padding-top: 200px;
  }

  .image {
    display: block;
    width: 100%;
    height: auto;
    z-index: -1;
    max-height: 100%;
    position: absolute;
    top: 0;
    left: 0;
  }

  .team {
    display: flex;
    flex-direction: column;
    padding: 40px;
    background-color: #fff;

    @media (max-width: $mobile-small) {
      padding: 20px;
    }

    .headline {
      font-weight: bold;
      margin-bottom: 20px;
      font-size: 3.2rem;
      text-transform: uppercase;
      .strike {
        text-decoration: line-through;
      }

      @media (max-width: $mobile-small) {
        font-size: 2.5rem;
      }
    }

    .subline {
      font-family: $font-mono;
      font-size: 1.2rem;
      margin-bottom: 80px;
      line-height: 1.5;
    }



    .member-filters {/*
      display: flex;
      flex-direction: row;
      justify-content: center;*/
      appearance: none;
      border: none;
      box-shadow: 2px 2px 5px 1px rgba(0,0,0,0.3);
      border-radius: 3px;
      margin: 25px;
      padding: 15px;

      option {
      }

      /*department-label {
        margin: 0 5px;
        background-color: #eee;
        padding: 2px 5px;

        label {
          display: block;
          user-select: none;
          padding: 10px;
        }

        input {
          display: none;
        }

        &.active {
          background-color: $color-orange;
          color: #fff;
        }
      }*/
    }

    .category {
      background-color: $color-blue;
      color: white;
      margin: 5px 20px;
      padding: 15px;
      width: 20%;
    }

    .member-grid {
      grid-template-columns: 1fr 1fr;
      grid-gap: 15px;
      margin: 40px 0;

      .member-item {
        width: 100%;
      }

      @media (min-width: $mobile-large) {
        display: grid;
      }
    }
  }
}

.image-footer {
  height: 50vh;
  background-size: cover;
  background-position: center;
}

</style>
