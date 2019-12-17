<template lang="pug">
    md-card
        md-card-header
            md-card-header-text Response:
        md-card-content
            .and-or
                .over-view
                    ResponseCard(v-for="(res, index) in response.over" :res="res" :title="index" :keyword="'over'")
                .under-view
                    ResponseCard(v-for="(res, index) in response.under" :res="res" :title="index" :keyword="'under'")
                .equals-view
                    ResponseCard(v-for="(res, index) in response['equal']" :res="res" :title="index" :keyword="'equal'")
            .true-and




</template>

<script>
    import ResponseCard from "./ResponseCard";
    export default {
        name: "ResponseView",
        components: {
            ResponseCard
        },
        props : {
            response: {}
        },

        computed: {
            trueAndFilter(){
                // let overEntries = [];
                // let equalEntries = [];
                // let underEntries = [];

                this.response.over.forEach(entry => {
                    console.log("this is object values in response.over: ");
                    console.log(Object.values(entry));
                    //overEntries.push(Object.values(entry))
                });

                this.response.equal.forEach(entry => {
                    console.log("this is object values in response.equal: ");
                    console.log(Object.values(entry));
                });

                this.response.under.forEach(entry => {
                    console.log("this is object values in response.under: ");
                    console.log(Object.values(entry));
                });

                return 1;
            }
        },


        methods: {

        }
    }
</script>

<style scoped lang="stylus">



</style>