<template>
    <div>
        <h3>RSS Feed</h3>
        <div v-html="rss"></div>
    </div>
</template>

<script>
    import DOMParser from "dom-parser"
    import axios from "axios";
    export default {
        props: ["blok"],
        data() {
            return {
            }
        },
        created() {
            console.log(this.$route);
        },
        computed: {
            rss() {
                console.log(this.$route.query.feed)
                return this.$route.query.feed;
            //return this.$store.dispatch('getRssFeed', this.$route.query.feed);
                //console.log(typeof this.$route.query.feed.rawHTML);
                /*var parser = new DOMParser();
                var doc = parser.parseFromString( this.$route.query.feed, "text/xml");
                console.log(doc);
                return doc;*/
            }
        },
        methods: {
        }
    }
</script>