<template>
    <div>
        <section class="">
            <product-item :blok="product.content"></product-item>
        </section>
    </div>
</template>

<script>
    export default {
        name: "shopItem",
        data() {
            return {
                title: 'Grand Garage Shop Item'
            }
        },
        computed: {
            product() {
                return this.$route.query.blok;
            }
        },
        head() {
            return {
                title: this.title,
                meta: [
                    {
                        hid: 'description',
                        name: 'description',
                        content: 'Grand Garage Makerspace Tabakfabrik Linz Shop Item',
                    }
                ]
            }
        }
    }
</script>

<style scoped>

</style>