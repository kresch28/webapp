<template>
    <div class="dashboard">
        <div class="infos">
            <div class="info-list">
                <div>
                    <h2>Willkommen in der Grand Garage</h2>
                </div>
                <div class="daily">
                    <li class="list-item">
                        <span><icon name="calendar" /></span>
                        <span class="text">{{dateFormat}}</span>
                    </li>
                    <li class="list-item">
                        <span><icon name="clock" /></span>
                        <span class="text">{{timeFormat}}</span>
                    </li>
                </div>
            </div>
        </div>
        <div class="spacer"></div>
        <div class="dashboard-block">
            <div class="infos">
                <div class="headline">
                    <h4>Links</h4>
                </div>
                <div class="shop-container">
                    <img src="~/assets/img/icons/idea.svg" class="links-icon-shop">
                    <div v-for="h, c in home" class="links-shop" v-if="c == 0">
                        <a :href="h.link.url" target="_blank">Grand Garage Wiki</a>
                    </div>
                </div>
                <div class="shop-container">
                    <img src="~/assets/img/icons/partner.svg" class="links-icon-partner">
                    <ul class="links-shop-list">
                        <li><a href="https://bit.ly/2G8mprC" target="_blank">Schachermayer</a></li>
                        <li><a href="https://bit.ly/2DHJ3W1" target="_blank">Haberkorn</a></li>
                        <li><a href="https://bit.ly/2Se2AFO" target="_blank">Kellner und Kunz</a></li>
                    </ul>
                </div>
                <div class="shop-container">
                    <img src="~/assets/img/icons/supermarket.svg" class="links-icon-shop">
                    <div class="links-shop">
                        <NuxtLink to= "order" target="_blank">Material bestellen</NuxtLink>
                    </div>
                </div>
            </div>
            <div class="faqs">
            </div>
        </div>
        <div class="workshop-overview">
            <h2 class="title-workshops">TIMETABLE WORKSHOPS</h2>
            <div class="workshop-list-wrapper">
                    <div v-if="workshops && workshops.length > 0" class="workshop-list">
                        <transition-group name="list">
                            <workshop-list-item
                                    v-for="item, c in workshops"
                                    :blok="item"
                                    :key="item.id"
                                    class="list-item"
                                    :slim="true"
                                    :date="date"
                                    v-if="c+1 <= (range*10)"
                            ></workshop-list-item>
                        </transition-group>
                        <button v-if="workshops.length > range*10" class="more" @click="more">more</button>
                    </div>
                <div v-else class="workshop-list-none">
                    <code>Keine Workshops für heute geplant</code>
                </div>
            </div>
        </div>
        <div class="machine-overview">
            <h2 class="title-machine">UNSERE MACHINEN</h2>
            <div class="machine-list-wrapper">
                <div v-if="machines && machines.length > 0" class="machine-list">
                    <transition-group name="list">
                        <machine-status-list-item  v-if="c+1 <= (range*10)" v-for="item, c in machines" :blok="item" :key="item.id" class="list-item"></machine-status-list-item>
                    </transition-group>
                </div>
                <div v-else class="machine-list-none">
                    <code>Keine Suchergebnisse</code>
                </div>
            </div>
            <button v-if="machines.length > range*10" class="more-machines" @click="more">more</button>
        </div>
    </div>
</template>

<script>
    import moment from "moment";

    export default {
        name: "dashboard",
        layout: 'screen',
        data () {
            return {
                date: new Date(),
                loading: false,
                search: '',
                workshops: [],
                machines: [],
                tags: [],
                range: 1,
            }
        },
        created() {
        },
        computed: {
            dateFormat() {
                const ye = new Intl.DateTimeFormat('de', { year: 'numeric' }).format(this.date)
                const mo = new Intl.DateTimeFormat('de', { month: 'short' }).format(this.date)
                const da = new Intl.DateTimeFormat('de', { day: '2-digit' }).format(this.date)

                return `${da}.${mo}.${ye}`;
            },
            timeFormat() {
                let dateFormat = this.date.getHours();
                if(this.date.getMinutes() < 10) {
                    dateFormat = dateFormat + ":0" + this.date.getMinutes();
                }
                else {
                    dateFormat = dateFormat + ":" + this.date.getMinutes();
                }

                return dateFormat;
            },
            selectedCategories() {
                return this.categories.filter((c) => {
                    return c.value;
                }).map((v) => {
                    return v.key;
                });
            },
            filters() {
                let filter_query = {
                    component: {
                        in: "workshop-date"
                    },
                    starttime: {
                        "gt-date": moment().format("YYYY-MM-DD HH:mm")
                    }
                };
                return {
                    filter_query,
                    search_term: this.search,
                }
            },
            home() {
                return this.$store.state.settings.home_navi;
            },

            lang() {
                // return '/'+this.$store.state.language+'/order';
                return '/order';
            },
        },
        methods: {
            update() {
                this.loading = true;
                let result = this.$store
                    .dispatch("findWorkshops", this.filters)
                    .then(data => {
                        this.loading = false;
                        this.workshops = data;
                    });
            },
            more() {
                this.range = this.range + 1;
            },
        },
        watch: {
            search() {
                this.update();
            }
        },
        async asyncData (context) {
            //let tags = await context.store.dispatch("loadTags");
            let filters = {
                filter_query: {
                    component: {
                        in: "workshop-date"
                    },
                    starttime: {
                        "gt-date": moment().subtract(24, "hours").format("YYYY-MM-DD HH:mm")
                    }
                }
            };
            let workshops = await context.store.dispatch("findWorkshops", filters).then((data) => {
                let res = { workshops: [] };
                if (data) {
                    //console.log(data);
                    for (let i = 0; i < data.length; i++) {
                        //console.log(data[i].dates);
                        for (let j = 0; j < data[i].dates.length; j++) {
                            let dt = data[i].dates[j].content.starttime.split('-', 3);
                            let mth = new Date().getMonth() + 1;
                            mth = '0' + mth;
                            if (dt[2].split(' ')[0] == new Date().getDate() && dt[1] == mth) {
                                res.workshops.push(data[i]);
                               //console.log('res ' + res);
                            } else {
                                // console.log(res);
                            }
                        }
                    }
                    // console.log(res.workshops)
                    return { workshops : res.workshops};
                }
                return { workshops: []};
            });

            let tags = await context.store.dispatch("loadTags");
            let filtersM = {
                filter_query: {
                    'component': {
                        'in': 'machine'
                    }
                }
            };
            let machines = await context.store.dispatch("findStatusMachines", filtersM).then((data) => {
                if (data.stories) {
                    //console.log(data.stories);
                    //console.log(data.stories);
                    return { machines: data.stories };
                }
                return { machines: [] };
            });
            return {tags, ...machines, ...workshops};

        },
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';

    .dashboard-block {
        display: flex;
        margin-bottom: 20px;
        @include media-breakpoint-down(sm) {
            flex-direction: column-reverse;
        }
        @include media-breakpoint-up(lg) {
            float: left;
            width: 20%;
        }
        align-items: flex-start;
        @include margin-page-middle();
        .infos {
            padding: 25px;

            .headline {
                background-color: rgb(0, 105, 170);
                color: #ffffff;
                font-weight: bold;
                padding: 5px;
                text-transform: uppercase;
                padding: 1px 10px;
            }

            background-color: #FFF;
            .info-list {
                li {
                    list-style: none;
                }
            }
        }

        .faqs {
            flex: 4;
            margin-bottom: 3vh;
        }
    }
    .spacer {
        margin: 40px;
    }
    .infos {
        padding: 25px;

        .headline {
            font-weight: bold;
            text-transform: uppercase;
        }

        background-color: #FFF;
        .info-list {
            @include media-breakpoint-up(sm) {
                display: flex;
                flex-direction: row;
                justify-content: space-between;
            }
            .daily {
                margin-top: 20px;
                width: 15%;
                @include media-breakpoint-down(sm) {
                    width: 60%;
                }
            }
            div {
                padding:0 20px;
            }
            li {
                list-style: none;
                svg {
                    width: 10%;
                }
                .text {
                    padding: 10px;
                }
            }
        }
    }
    .workshop-overview {
        display: flex;
        flex-direction: column;
        align-items: center;
        .loading {
            position: absolute;
            left: 50%;
            transform: translate(-50%, -40px);
        }

        .workshop-filters {
            .filters {
                background-color: $color-orange;
                display: flex;
                .tags {
                    flex: 3;
                }
                .calendar {
                    flex: 1;
                    max-width: 320px;
                    .reset {
                        margin-top: -3px;
                        background-color: #000;
                        padding: 10px;
                        .all {
                            padding: 10px;
                            color: #FFF;
                            &:hover {
                                cursor: pointer;
                                color: 000;
                                background-color: $color-yellow;
                            }
                        }
                    }
                }
            }
            .tags {
                padding-bottom: 4vh;
                @include media-breakpoint-down(sm) {
                    padding: 4vh 0;
                }
                .headline {
                    padding-top: 4vh;
                    color: #FFF;
                    font-weight: bold;
                    font-size: 1.8rem;
                    @include margin-page-wide();
                    margin-bottom: 20px;
                    text-transform: uppercase;
                    letter-spacing: .05em;
                    @include media-breakpoint-down(sm) {
                        font-size: 1.2rem;
                        margin-bottom: 10px;
                    }
                }
                .tag-list {
                    @include margin-page-wide();
                    display: grid;
                    max-width: 70em;
                    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
                    @include media-breakpoint-down(lg) {
                        grid-template-columns: 1fr 1fr 1fr 1fr;
                    }
                    @include media-breakpoint-down(md) {
                        grid-template-columns: 1fr 1fr 1fr;
                    }
                    @include media-breakpoint-down(sm) {
                        grid-template-columns: 1fr 1fr;
                        font-size: .85em;
                    }
                    @include media-breakpoint-down(xs) {
                        grid-template-columns: 1fr;
                    }
                    grid-gap: 15px 20px;
                    >.tag {
                        font-family: $font-mono;
                        color: #FFF;
                        user-select: none;
                        cursor: pointer;
                        input[type=checkbox] {
                            outline: none;
                            -webkit-appearance: none;
                            padding: 5px;
                            border: 1px solid #FFF;
                            border-radius: 3px;
                            position: relative;
                            top: 0;
                            &:checked {
                                background-color: #FFF;
                            }
                        }
                    }
                }
                @include media-breakpoint-down(sm) {
                    overflow: hidden;
                    position: relative;
                    max-height: 1000px;
                    transition: all .3s linear;
                    padding-bottom: 30px;
                    .expander {
                        cursor: pointer;
                        position: absolute;
                        bottom: 0;
                        width: 100%;
                        height: 20px;
                        transition: all .3s linear;
                        &:after {
                            transition: all .3s linear;
                            content: "";
                            position: absolute;
                            bottom: 18px;
                            left: 50%;
                            width: 10px;
                            height: 10px;
                            bottom: 8px;
                            border-bottom: 2px solid #fff;
                            border-right: 2px solid #fff;
                            margin-left: -13px;
                            transform: rotate(225deg);
                            transform-origin: center center;
                        }
                    }
                    &.collapsed {
                        max-height: 17vh;
                        .expander {
                            height: 70px;
                            background: linear-gradient(rgba(0,0,0,0), $color-orange 80%);
                            &:after {
                                transform: rotate(45deg);
                                bottom: 18px;
                            }
                        }
                    }
                }
            }

            .search {
                display: flex;
                margin: 0 4%;
                padding-top: 1rem;
                padding-bottom: 4rem;
                input[type="text"] {
                    flex: 1;
                    display: block;
                    width: 100%;
                    padding: 10px;
                    outline: none;
                    font-family: $font-secondary;
                    font-size: 1.1rem;
                    border: none;
                }
                input[type="button"] {
                    font-size: 1.1rem;
                    margin-left: 10px;
                    text-transform: uppercase;
                    background-color: transparent;
                    border: none;
                    font-weight: bold;
                    color: $color-orange;
                    outline: none;
                }
            }
        }
        .workshop-list-wrapper {
            margin: 0 4%;
            display: flex;
            .workshop-list {
                flex: 3;
                .list-item {
                    margin-right: 10px;
                }
                .list-enter-active,
                .list-leave-active {
                    transition: all 0.5s;
                }
                .list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
                    opacity: 0;
                    transform: translateX(30px);
                }
            }
            .workshop-list-none {
                flex: 3;
                text-align: center;
            }
        }
    }
    .more {
        background-color: #ff6f00;
        border: 1px solid #ff8c33;
        color: #FFF;
        cursor: pointer;
        left: 50%;
        line-height: 1;
        margin-top: 40px;
        outline: none;
        padding: 7px 12px 8px;
        position: absolute;
        right: 50%;
        @include media-breakpoint-down(sm) {
            left: 40%;
        }
    }

    .more-machines {
        background-color: #ff6f00;
        border: 1px solid #ff8c33;
        color: #FFF;
        cursor: pointer;
        left: 50%;
        line-height: 1;
        margin-top: 20px;
        margin-bottom: 40px;
        outline: none;
        padding: 7px 12px 8px;
        position: absolute;
        right: 50%;
        @include media-breakpoint-down(sm) {
            left: 40%;
        }
    }
    .links-icon {
        border-radius: 50%;
        height: 18%;
        width: 18%;
        padding: 5px;
        float: left;
    }

    .links-partner {
        background-color: #ebe223;
        border-radius: 50%;
        height: 18%;
        width: 18%;
        padding: 10px;
        float: left;
    }

    .links-wiki {
        position: absolute;
        margin-top: 1%;
        margin-left: 5%;
        @include media-breakpoint-down(sm){
            margin-top: 4%;
            margin-left: 25%;
        }
    }
    .links-icon-small {
        border-radius: 50%;
        height: 20%;
        width: 20%;
        padding: 5px;
        float: left;
        margin-top: 35px;
    }

    .links-icon-partner {
        border-radius: 50%;
        height: 18%;
        width: 18%;
        padding: 10px;
        float: left;
        margin-top: 15px;
    }

    .links-icon-shop {
        border-radius: 50%;
        height: 18%;
        width: 18%;
        padding: 10px;
        float: left;
    }

    .shop-container {
        float: left;
        display: flex;
        margin-top: 30px;
        font-size: 22px;
        /*@include media-breakpoint-down(md) {
        margin-top: 60px;
        }
        @include media-breakpoint-down(sm) {
            margin-top: 0;
        }*/
    }

    .links-shop {
        margin-left: 40px;
        margin-top: 4%;
        @include media-breakpoint-down(md) {
            margin-left: 80px;
            margin-top: 30px;
        }
        @include media-breakpoint-down(sm) {
            margin-left: 40px;
            margin-top: 15px;
        }
    }

    .links-shop-list {
        list-style: none;
        padding: 0;
        margin-left: 40px;
        margin-top: 0px;
        @include media-breakpoint-down(md) {
            margin-left: 80px;
            margin-top: 30px;
        }
        @include media-breakpoint-down(sm) {
            margin-left: 40px;
            margin-top: 15px;
        }
        li {
            padding: 5px;
        }
    }

    .title-workshops {
        text-align: center;
    }

    .title-machine {
        text-align: center;
    }

    .machine-list-wrapper {
        display: flex;
        @include margin-page-wide();
        .machine-list {
            > span {
                @include media-breakpoint-up(sm) {
                    display: grid;
                    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
                }
            }
            flex: 3;
            .list-item {
                min-width: 150px;
                padding: 0 30px;
                .machine-list-item {
                    margin-bottom: 0;
                }
            }
            .list-enter-active, .list-leave-active {
                transition: all 0.5s;
            }
            .list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
                opacity: 0;
                transform: translateX(30px);
            }
        }
        .machine-list-none {
            flex: 1;
            text-align: center;
        }
    }
    .machine-overview {
        margin-top: 120px;
        position: absolute;
        width: 65%;
        right: 0;
    }
</style>
