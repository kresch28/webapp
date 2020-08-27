<template>
    <div v-editable="machine" class="machine-page" v-if="machine">
        <machine-header :story="story"></machine-header>
        <div class="machine-teaser">
            <div class="body">
                <div class="image">
                    <img :src="$resizeImage(machine.image, '700x0')" alt=""/>
                </div>
                <div class="description text">
                    <markdown :value="machine.details"></markdown>
                </div>
            </div>
        </div>
        <div class="body">
            <div class="inner-body">
                <!--
                <div class="description">
                  <markdown :value="machine.description"></markdown>
                </div>
                -->
                <div class="machine-list" v-if="hasUser">
                    <div class="machine-item" v-for="m in machine.machine_status_items">
                    </div>
                </div>
                <div v-else class="machine-list">
                    <div class="machine-list-warning">
                        Du musst angemeldet sein um die Verfügbarkeit der Maschinen sehen zu können!
                    </div>
                </div>
            </div>
        </div>
        <div class="body">
            <image-slideshow :blok="images"></image-slideshow>
        </div>
        <div class="body" v-if="machine.links && machine.links.length > 0">
            <h3 class="blue">Links</h3>
            <ul class="link-list">
                <li class="link-item" v-for="(i, index) in machine.links" v-bind:key="index">
                    <div class="title">
                        {{i.title}}
                    </div>
                    <a class="url" :href="i.url" target="_blank">{{i.url}}</a>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: "ProductItem",
        props: ['story'],
        computed: {
            machine() {
                return this.story.content;
            },
            tags() {
                return this.story.tag_list;
            },
            hasUser() {
                return !!this.$store.state.user;
            },
            images() {
                return {
                    items: this.machine.images
                }
            }
        }
    }
</script>

<style scoped>

</style>