<template>
    <div>
        <h4 class="headline">Einkaufswagen</h4>
        <div class="basket-info">
            <div class="basket-menu">
                <NuxtLink to="/order">Weiter einkaufen</NuxtLink>
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
                <button class="delete">x</button>
                <div class="item">
                    <div class="item-image">
                        <img :src="$resizeImage(product.image, '150x0')" alt=""/>
                    </div>
                    <div class="item-info">
                        <span>{{product.title}}</span>
                    </div>
                    <div class="item-amount">
                        <span>
                            <select class="amount select" name="amount" id="amount">
                                <option value="1" v-model="amount">1</option>
                                <option value="2" v-model="amount">2</option>
                                <option value="3" v-model="amount">3</option>
                                <option value="4" v-model="amount">4</option>
                                <option value="5" v-model="amount">5</option>
                                <option value="6" v-model="amount">6</option>
                                <option value="7" v-model="amount">7</option>
                                <option value="8" v-model="amount">8</option>
                                <option value="9" v-model="amount">9</option>
                                <option value="10" v-model="amount">10</option>
                            </select>
                        </span>
                    </div>
                    <div class="item-price">
                        <span>{{product.title}}</span>
                    </div>
                </div>
            </div>
        </div>
        <div class="basket-order-info">
            <div class="basket-details">
                <span class="items"></span>
                <span class="amount"><b>Total</b></span>
                <span class="price">..€</span>
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

    .basket-order-info {
        margin-left: auto;
        margin-right: auto;
        width: 80%;
        border-top: 1px solid $color-orange;
        padding-top: 5px;

        .basket-details {
            .price {
                flex: 1;
                text-align: right;
                margin-bottom: 10px;

                @include media-breakpoint-down(sm) {
                    font-weight: bold;
                }
            }
        }
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
        position: relative;
        @include media-breakpoint-up(sm) {
            padding: 20px;
        }

        .delete {
            border: 2px solid #000000;
            border-radius: 50%;
            float: left;
            margin-left: -60px;
            position: absolute;
            top: 45%;
        }

        .item {
            display: flex;
            margin-left: -20px;
            align-items: center;

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
                margin-right: 90px;
                span {
                    @include media-breakpoint-down(sm) {
                        display: none;
                    }
                }
            }

            .item-price {
                @include media-breakpoint-up(sm) {
                    flex: 1;
                    text-align: right;
                }
                @include media-breakpoint-down(s) {
                    flex: 2;
                }
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
