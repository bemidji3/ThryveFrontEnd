<template lang="pug">
    .container
        md-card
            md-card-header
                md-card-header-text Select filters and press red check box to confirm your filter.  You can delete filters with the red trashcan button, but only after the filter has been confirmed:
            md-card-content
                FilterCard(v-for="(filter, index) in filters" @confirmError="displayErrorDialog" @confirm="addToFilters" @delete="deleteFilter" :id="index" :filter="filter" v-if="filter.deleted === false")
            md-card-actions
                md-button(@click="addNewFilter").md-primary.md-raised Add Filter
                md-button(@click="sendRequest" :disabled="!ableToSubmit").md-accent.md-raised Submit


</template>



<script>

    //5 food group for categories
    //  (1)


    import FilterCard from './FilterCard.vue'
    export default {
        name: "FilterView",
        data () { //global variables
            return {
                filters: [],
                numberOfFilters: 0,
                errorDialog: false,
                masterFilterObject: {}
            }
        },

        computed: { //variables that are going to be dynamic/changing
            ableToSubmit(){
                let returnValue = true;

                this.filters.forEach(filter => {
                    if(filter.confirmed !== true){
                        returnValue = false;
                    }
                });

                return returnValue
            }
        },

        components: {
            FilterCard
        },

        mounted() {
            this.createMasterFilterObject();
        },

        methods: {

            displayErrorDialog(){
                console.log("filterview emitting confirm error ");
                this.$emit('confirmError')
            },


            deleteFilter(id, filterArray){
                let masterKey = filterArray[0];
                let keyValuePair = filterArray[1];

                console.log("deleting this id: " + id);
                this.filters[id-1].deleted = true;


                let invidualKey = Object.keys(keyValuePair);


                delete this.masterFilterObject[masterKey][invidualKey];

                console.log("this is my filters object: ");
                console.log(this.filters)

                this.numberOfFilters--;

            },

            async sendRequest(){
                const myUrl = "http://127.0.0.1:5000/food/return";

                console.log("this is my master filter object");
                console.log(this.masterFilterObject);

                const options = {
                    method: "POST",
                    headers : {
                        'Content-Type' : 'application/json'
                    },
                    body: JSON.stringify(this.masterFilterObject)

                };

                const myRequest = await fetch(myUrl, options);
                const myResponse = await myRequest.json();
                console.log(myResponse);

                this.$emit('gotResponse', myResponse);

            },
            createMasterFilterObject() {
                this.masterFilterObject.over = {};
                this.masterFilterObject.equal = {};
                this.masterFilterObject.under = {};
            },

            addNewFilter(){
                this.numberOfFilters++;
                const newFilterObject = {
                    'key' : this.numberOfFilters,
                    'deleted': false,
                    'confirmed' : false
                };
                this.filters.push(newFilterObject);
            },

            addToFilters(val){

                //console.log("adding this value to filters: ");
                //console.log(val);



                //this.filters.push(val);

                switch (val[0]) {
                    case "equal":
                        this.masterFilterObject.equal = {...this.masterFilterObject.equal, ... val[Object.keys(val)[1]]};
                        break;
                    case "under":
                        this.masterFilterObject.under = {...this.masterFilterObject.under, ... val[Object.keys(val)[1]]};
                        break;
                    case "over":
                        this.masterFilterObject.over = {...this.masterFilterObject.over, ... val[Object.keys(val)[1]]};
                        break;
                }





            }
        }
    }
</script>

<style scoped lang="stylus">


</style>