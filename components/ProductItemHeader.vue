<template>
    <div class="machine-header">
        <div class="header-image" :style="{ 'background-image': 'url(' + $resizeImage(machine.headerImage, '1600x0') + ')' }">
        </div>
        <div class="header-title" v-if="machine.title">
            <div class="tags">
        <span v-for="(tag, index) in tags">
          {{tag}}<span v-if="index+1 < tags.length">, </span>
        </span>
            </div>
            <div class="title">{{machine.title}}</div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "ProductItemHeader",
        props: ['story'],
        computed: {
            machine() {
                return this.story.content;
            },
            tags() {
                return this.story.tag_list;
            },
        }
    }
</script>

<style scoped>

</style>