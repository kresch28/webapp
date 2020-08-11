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
                </div>
            </div>
            <div class="faqs">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "dashboard",
        data () {
            return {
                date: new Date(),
            }
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
            }
        }
    }
</script>

<style lang="scss">
    @import '@/assets/scss/styles.scss';

    .dashboard-block {
        display: flex;
        @include media-breakpoint-down(sm) {
            flex-direction: column-reverse;
        }
        align-items: flex-start;
        @include margin-page-middle();
        .infos {
            padding: 25px;
            flex: 1;

            .headline {
                font-weight: bold;
                text-transform: uppercase;
                margin-bottom: .8em;
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
        flex: 1;

        .headline {
            font-weight: bold;
            text-transform: uppercase;
            margin-bottom: .8em;
        }

        background-color: #FFF;
        .info-list {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            .daily {
                margin-top: 20px;
                width: 15%;
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

</style>