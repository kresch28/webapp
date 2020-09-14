<template>
    <div>
        <h4 class="headline">Einkaufswagen</h4>
        <div class="basket-info">
            <div class="basket-menu">
                <a href="/order">Weiter Einkaufen</a>
                <button class="button">Bestellen</button>
            </div>
            <div class="basket-details">
                <span class="items">Produkte</span>
                <span class="amount">Menge</span>
                <span class="price">Preis</span>
            </div>
        </div>
        <div>
            <div class="item-container">
                <div class="item">
                    <div class="item-image">
                        <img :src="$resizeImage(product.image, '150x0')" alt=""/>
                    </div>
                    <div class="item-info">
                        <span>{{product.title}}</span>
                    </div>
                    <div class="item-amount">
                        <span>{{product.title}}</span>
                    </div>
                    <div class="item-price">
                        <span>{{product.title}}</span>
                    </div>
                </div>
            </div>
        </div>
        <div class="basket-footer">
            <div class="basket-menu">
                <a></a>
                <button class="button">Bestellen</button>
            </div>
        </div>
    </div>
    
</template>

<script>
    export default {
        name: "basket",
        props: ['blok'],
        computed: {
            product() {
                console.log(this.$route);
                return this.$route.query.product;
            }
        }
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';
    .headline {
        color: #fff;
        background-color: $color-blue;
        font-weight: 700;
        font-size: 1.8rem;
        margin-bottom: 20px;
        padding: 10px;
        text-transform: uppercase;
        letter-spacing: .05em;
        width: 20%;
        text-align: center;

        @include media-breakpoint-up(sm) {
            margin-left: 4%;
            margin-right: 4%;
        }

        @include media-breakpoint-down(sm){
            margin-left: auto;
            margin-right: auto;
            width: 80%;
        }
    }

    .basket-info {
        margin-left: auto;
        margin-right: auto;
        width: 80%;
        border-bottom: 1px solid $color-orange;
        padding-bottom: 5px;
    }

    .basket-details {
        margin-top: 20px;
        display: flex;

        .items {
            flex: 2;
        }

        .amount {
            flex: 1;
            text-align: right;
            @include media-breakpoint-down(sm) {
                display: none;
            }
        }

        .price {
            flex: 1;
            text-align: right;
        }
    }

    .basket-menu {
        display: flex;
        justify-content: space-between;
        margin-left: auto;
        margin-right: auto;

        a {
            @include media-breakpoint-up(sm) {
                padding-top: 18px;
            }
            @include media-breakpoint-down(sm) {
                padding-top: 10px;
            }
        }

        button {
            cursor: pointer;
            font-weight: bold;
            padding: 10px;
            border: none;
            outline: none;
            color: #FFF;
            background-color: $color-orange;
            margin-bottom: 12px;
        }
    }

    .item-container {
        padding-top: 20px;
        padding-bottom: 20px;
        margin-right: auto;
        margin-left: auto;
        width: 80%;
        @include media-breakpoint-up(sm) {
            padding: 20px;
        }

        .item {
            display: flex;

            .item-image {
                flex: 1;
                @include media-breakpoint-down(sm) {
                    display: none;
                }
            }

            .item-info {
                flex: 2;
            }

            .item-amount {
                flex: 1;
                text-align: right;
                span {
                    @include media-breakpoint-down(sm) {
                        display: none;
                    }
                }
            }

            .item-price {
                flex: 1;
                text-align: right;
            }
        }
    }

    .basket-footer {
        margin-left: auto;
        margin-right: auto;
        width: 80%;
        padding-bottom: 5px;
    }


</style>
