<template>
    <div>
        <nuxt-link :to="{ path: '/shopItem', query: { blok }}">
            <div class="product-list-item">
                <div class="image">
                    <img :src="$resizeImage(content.image, '350x350')" alt=""/>
                </div>
                <div class="body">
                    <div class="title">
                        {{content.title}}
                    </div>
                    <div class="price">
                        {{price}} €
                    </div>
                    <div class="teaser">
                        <markdown :value="content.teaser"></markdown>
                    </div>
                    <button v-if="!id">Mehr</button>
                    <button v-if="id" v-bind:class="{ isSold: id }">Ausverkauft</button>
                </div>
            </div>
        </nuxt-link>
        <!--<div class="product-list-item" v-else>
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
        </div>-->
    </div>
</template>

<script>
    export default {
        name: "ProductListItem",
        props: ['blok', 'key'],
        computed: {
            content() {
                console.log(this.blok)
                return this.blok.content;
            },
            id() {
                return this.blok.tag_list[0]=='CNC-Werkstatt';
            },
            price() {
                let price = this.blok.id;
                let digit = (''+price)[1] + (''+price)[2];
                if(digit < 10) {
                    console.log((''+price)[1] + (''+price)[2]);
                }
                return (''+price)[1] + (''+price)[2];
            }
        }
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';

    .product-list-item {
        .body {
            padding: 10px;
        }
        .title {
            padding-top: 10px;
        }

        .price {
            padding: 10px 0;
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

        button.isSold {
            cursor: pointer;
            font-weight: bold;
            padding: 10px;
            border: none;
            outline: none;
            color: #FFF;
            background-color: #808080;
        }

    }

</style>
