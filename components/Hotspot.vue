<template>
    <div v-editable="blok" class="hotspot">
        <div class="hotspot-wrapper">
            <img class="picture" v-if="blok.image" :src="$resizeImage(blok.image, '700x700')"/>
            <span class="title"><b>{{blok.title}}</b></span>
            <span class="description">{{blok.description}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Hotspot",
        props: ['blok'],
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';

    .hotspot {

        .hotspot-wrapper {
            display: flex;
            flex-direction: column;

            .picture {
                height: 50%;
                width: 50%;
            }
            .title {
                margin-top: 1em;
                font-family: $font-secondary;
                font-size: 1.5rem;
                margin-bottom: 1em;
            }
            .description {
                line-height: 1.4;
                font-size: 0.8rem;
                font-family: "Roboto Mono", monospace;
            }
        }
    }


</style>