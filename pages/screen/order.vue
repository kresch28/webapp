<template>
    <!--<component v-if="story && story.content && story.content.component" :key="story.content._uid" :blok="story.content" :is="story.content.component"></component>
    -->
    <div class="order-wrapper">
        <div class="order-container info">
            <h4 class="headline">Materialien bestellen</h4>
            <div class="filter">
                <div class="filter-producer">
                    <span>Händler</span>
                    <div>
                        <input type="checkbox" id="producer1" name="producer1" value="CNC-Werkstatt" v-model="producer">
                        <label for="producer1">Tiger-Coatings</label><br>
                    </div>
                </div>
                <div class="filter-price">
                    <span>Preisrahmen</span>
                    <div>
                        <input type="checkbox" id="price1" name="price1" value="1" v-model="price">
                        <label for="price1">1€ - 10€</label><br>
                        <input type="checkbox" id="price2" name="price2" value="2" v-model="price">
                        <label for="price2">10€ - 50€</label><br>
                        <input type="checkbox" id="price3" name="price3" value="3" v-model="price">
                        <label for="price3">50€ - 100€</label><br>
                    </div>
                </div>
                <div class="filter-price">
                    <span>Bereich</span>
                        <ul class="filter-category">
                            <li v-for="t, c in tags">
                                <input v-bind:id="tags[c].name" type="checkbox" v-model="category" v-bind:value="tags[c].name" />
                                <label v-bind:for="tags[c].name">{{tags[c].name}}</label>
                            </li>
                        </ul>
                </div>
                <input class="filter-button" type="button" value="Filtern" v-on:click="producers">
            </div>
        </div>
        <div class="order-container content">
            <div class="order-search">
                <input class="order-search-input" type="text" v-model="search" name="" id="" placeholder="Suche">
                <!--<img src="~/assets/img/icons/search.svg" class="search-icon">-->
            </div>
            <div class="order-content-info">
                <div v-if="range > articles">1 - {{articles}}  von {{articles}} Artikel</div>
                <div v-else>1 - {{range}}  von {{articles}} Artikel</div>
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
            <div v-if="articles > range" class="button-wrapper">
                <button v-on:click="more">Mehr laden</button>
            </div>
        </div>
    </div>
</template>

<script>
    import storyblokLivePreview from '@/mixins/storyblokLivePreview'


    export default {
        middleware: 'authenticated',
        layout: 'screen',
        mixins: [storyblokLivePreview],
        data () {
            return {
                products: [],
                tags: [],
                range: 15,

                loading: false,
                search: '',
                sort: '',
                producer: [],
                category: [],
                price: [],
                priceRange: {
                    1: [0,10],
                    2: [10,50],
                    3: [50,100]
                },
                productReturn : [],
                amount: '',
                tagsCollapsed: true,
            }
        },
        created() {
            console.log(this.products);
            console.log(this.category);
            this.$watch('tags', (newVal, oldVal) => {
                this.update();
            }, { deep: true });
        },
        watch: {
            search() {
                this.update();
            },
        },
        methods: {
            more() {
                /*this.showMore[z] = this.range;
                console.log(this.showMore);*/
                this.range = this.range + 15;
                console.log(this.range);
            },
            update() {
                this.loading = true;
                let result = this.$store.dispatch("findMachines", this.filters).then((data) => {
                    this.loading = false;
                    this.products = data.stories;
                });
            },
            toggleTags() {
                this.tagsCollapsed = !this.tagsCollapsed;
            },
            setFilter(filter) {
                if (this.filterApplied.indexOf(filter) > -1) {
                    this.filterApplied.pop(filter);
                } else {
                    this.filterApplied.push(filter);
                }
                console.log(this.filterApplied);
            },
            clearFilter(){
                this.filterApplied = []
            },
            producers() {
                /*return this.products.filter(function (p) {
                    return this.producer.includes(p.tag_list[0]);
                }, this);*/

                console.log(this.producer);
                console.log(this.price);
                console.log(this.category);

                if(this.producer.length > 0) {
                    this.products = this.products.filter(function (p) {
                        return this.producer.includes(p.tag_list[0]);
                    }, this);
                }

                if(this.price.length > 0){
                    console.log('in');
                    for(let j = 0; j < this.products.length; j++) {
                        let low;
                        let high;
                        for (let i = 0; i < this.price.length; i++) {
                            low = this.priceRange[this.price[i]][0];
                            high = this.priceRange[this.price[i]][1]
                        }
                        let productReturnTo = (''+this.products[j].id)[1] + (''+this.products[j].id)[2];
                        if (productReturnTo >= low && productReturnTo <= high) {
                            console.log(this.products[j]);
                            this.productReturn.push(this.products[j])
                        }
                        console.log(this.productReturn);
                    }
                    this.products = this.productReturn;
                }

                if(this.category.length > 0){
                    this.products = this.products.filter(function (p) {
                        return this.category.includes(p.tag_list[0]);
                    }, this);
                }
                /*
                let price = [];
                for(let i = 0; i < this.products.length; i++){
                    price = (''+this.products[i].id)[1] + (''+this.products[i].id)[2];
                }
                this.products = this.products.filter(function (p) {
                    return this.price  
                })
                return (''+price)[1] + (''+price)[2];
                */

                console.log(this.products);

                /*this.search = this.producer;
                this.update();*/
            },
        },
        computed: {
            user() {
                return this.$store.state.user;
            },
            articles(){
                return this.products.length;
            },
            items() {
                console.log(this.products);
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
        @include media-breakpoint-up(sm){
            display: flex;
            flex-direction: row;
        }

        .order-container.info {
            @include media-breakpoint-up(sm){
                width: 20%;
                height: 90vh;
            }
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
                width: 90%;
                text-align: left;
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
                    .filter-category {
                        margin-left: -38px;
                        li {
                            list-style: none;
                        }
                    }
                }

                .filter-button {
                    cursor: pointer;
                    font-weight: bold;
                    padding: 10px 20px;
                    border: none;
                    outline: none;
                    color: #FFF;
                    background-color: $color-orange;
                    margin-bottom: 15px;
                }
            }
        }

        .order-container.content {
            @include media-breakpoint-up(sm){
                width: 80%;
            }

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
                margin-top: 15px;
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
