<template>
    <div>
        <!--<nuxt-link :to="'/' + blok.full_slug" v-if="content.hasContent">-->
            <div class="product-list-item">
                <div class="image">
                    <img :src="$resizeImage(content.image, '300x300')" alt=""/>
                </div>
                <div class="body">
                    <div class="title">
                        {{content.title}}
                    </div>
                    <div class="price">
                        €
                    </div>
                    <div class="teaser">
                        <markdown :value="content.teaser"></markdown>
                    </div>
                    <button>Mehr</button>
                </div>
            </div>
        <!--</nuxt-link>-->
        <!--<div class="product-list-item" v-else>-->
        <div class="product-list-item">
            <div class="image">
                <img :src="$resizeImage(content.image, '300x300')" alt=""/>
            </div>
            <div class="body">
                <div class="title">
                    {{content.title}}
                </div>
                <div class="price">
                    €
                </div>
                <div class="teaser">
                    <markdown :value="content.teaser"></markdown>
                </div>
                <button>Mehr</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "ProductListItem",
        props: ['blok'],
        computed: {
            content() {
                return this.blok.content;
            },
        }
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';

    .product-list-item {
        margin: 20px 10px;
        padding: 20px 40px;
        .title {
            padding: 10px;
        }

        .price {
            padding: 10px;
        }
        button {
            cursor: pointer;
            font-weight: bold;
            padding: 10px;
            border: none;
            outline: none;
            color: #FFF;
            background-color: $color-orange;
        }

    }

</style>