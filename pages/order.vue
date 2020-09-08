<template>
    <!--<component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component"></component>
    -->
    <div class="order-wrapper">
        <div class="order-container info">
            <h4 class="headline">Materialien bestellen</h4>
            <div class="filter">
                <div class="filter-producer">
                    <span>Hersteller</span>
                    <div>
                        <input type="checkbox" id="producer1" name="producer1" value="Tiger-Coatings">  <!--v-model="producer"-->
                        <label for="producer1">Tiger-Coatings</label><br>
                    </div>
                </div>
                <div class="filter-price">
                    <span>Preisrahmen</span>
                    <div>
                        <input type="checkbox" id="price1" name="price1" value="10">
                        <label for="price1">1€ - 10€</label><br>
                        <input type="checkbox" id="price2" name="price2" value="20">
                        <label for="price2">10€ - 50€</label><br>
                        <input type="checkbox" id="price3" name="price3" value="30">
                        <label for="price1">50€ - 100€</label><br>
                    </div>
                </div>
                <div class="filter-price">
                    <span>Kategorie</span>
                    <div v-for="t, c in tags" :key="c">
                        <input type="checkbox" :id="c" v-model="tags[c]">
                        <label :for="c">{{tags[c].name}}</label><br>
                    </div>
                </div>
            </div>
        </div>
        <div class="order-container content">
            <div class="order-search">
                <input class="order-search-input" type="text" v-model="search" name="" id="" placeholder="Suche">
                <!--<img src="~/assets/img/icons/search.svg" class="search-icon">-->
            </div>
            <div class="order-content-info">
                <div>1 - {{range}}  von {{articles}} Artikel</div>
                <div>
                    <select name="sort" id="sort">
                        <option value="none">Sortieren nach</option>
                        <option value="priceUp">Preis aufsteigend</option>
                        <option value="priceDown">Preis absteigend</option>
                    </select>
                </div>
            </div>
            <div class="products-list-wrapper">
                <div v-if="items && items.length>0" class="products-list">
                    <!--<transition-group name="list">-->
                    <product-list-item v-for="item, c in items" v-if="c <= range" :blok="item" :key="item.id" class="list-item"></product-list-item>
                    <!--</transition-group>-->
                </div>
            </div>
            <div class="button-wrapper">
                <button>Mehr laden</button>
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
                range: 15,

                loading: false,
                search: '',
                tagsCollapsed: true,
            }
        },
        created() {
            console.log(this.products);
            this.$watch('tags', (newVal, oldVal) => {
                this.update();
            }, { deep: true });
        },
        watch: {
            search() {
                this.update();
            },
            producer() {

            }
        },
        methods: {
            update() {
                this.loading = true;
                let result = this.$store.dispatch("findMachines", this.filters).then((data) => {
                    this.loading = false;
                    this.products = data.stories;
                });
            },
            toggleTags() {
                this.tagsCollapsed = !this.tagsCollapsed;
            }
        },
        computed: {
            user() {
                return this.$store.state.user;
            },
            articles(){
                return this.products.length;
            },
            items() {
                console.log(this.tags);
                return this.products;
            },
            filters() {
                return {
                    filter_query: {
                        'component': {
                            'in': 'machine'
                        }
                    },
                    search_term: this.search,
                    with_tag: this.filterTags.join(',')
                }
            },
            filterTags() {
                return this.tags.filter((t) => {
                    return t.value;
                }).map((t) => {
                    return t.name;
                });
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
    .order-wrapper {
        display: flex;
        flex-direction: row;

        .order-container.info {
            width: 20%;
            border: 1px solid $color-blue;
            margin-top: 35px;

            .headline {
                color: #fff;
                background-color: $color-blue;
                font-weight: 700;
                font-size: 1.8rem;
                margin-left: 4%;
                margin-right: 4%;
                margin-bottom: 20px;
                padding: 5px;
                text-transform: uppercase;
                letter-spacing: .05em;
            }

            .filter {
                margin-left: 20px;
                margin-top: 60px;
                .filter-producer {
                    span {
                        color: #ff6f00;
                        margin: 10px 0;
                    }
                    div {
                        margin: 20px 0;
                    }

                }

                .filter-price {
                    span {
                        color: #ff6f00;
                        margin: 10px 0;
                    }
                    div {
                        margin: 20px 0;
                    }

                }
            }
        }

        .order-container.content {
            width: 80%;


            .order-search {
                display: flex;
                justify-content: space-between;
                width: 89%;
                margin-left: auto;
                margin-right: auto;
                margin-top: 40px;
                margin-bottom: 20px;

                .order-search-input{
                    width: 100%;
                    background-color: $color-bright-bg;
                    border: 1px solid $color-blue;
                    font-size: medium;
                    padding: 5px;
                }
            }

            .order-content-info {
                display: flex;
                justify-content: space-between;
                width: 89%;
                margin-left: auto;
                margin-right: auto;
                margin-top: 40px;
                margin-bottom: 20px;

                #sort {
                padding: 5px;
                font-size: medium;
            }

            }
            .products-list-wrapper {
                width: 100%;
                margin-left: auto;
                margin-right: auto;

                .products-list {
                    display: flex;
                    flex-wrap: wrap;
                    justify-content: space-evenly;
                }

                .list-item {
                    background-color: #ffffff;
                    border: 1px solid #d3d3d3;
                    border-radius: 5px;
                    margin: 10px 20px;
                    @include media-breakpoint-down(md){
                        width: 50%;
                    }
                    @include media-breakpoint-down(sm){
                        width: 100%;
                    }
                }
            }
            .button-wrapper {
                width: 12%;
                margin-left: auto;
                display: flex;
                flex-direction: column;
                /* justify-content: center; */
                margin-right: auto;
                text-align: center;

                .button-info {
                    margin-bottom: 10px;
                    margin-top: 20px;
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
        }
    }

    .fontAwesome{ font-family: 'Helvetica', FontAwesome, sans-serif; }
</style>
