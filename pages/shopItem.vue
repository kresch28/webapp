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
        computed: {
            product() {
                console.log(this.$route);
                return this.$route.query.blok;
            }
        }
    }
</script>

<style scoped>

</style>