<template lang="pug">
    md-card
        md-card-content.md-layout
            .dropdown-menu.md-size-30.md-layout-item
                md-field.md-layout-item
                    label Nutrient Choice
                    md-select(v-model="nutrientChoice")
                        md-option(value="203") Protein
                        md-option(value="204") Fat
                        md-option(value="205") Carbohydrate
                        md-option(value="269") Sugar
            .buttons.md-size-30.md-layout-item.md-alignment-center-center.md-layout
                label Filters:
                .inner-buttons-div
                    md-radio(v-model="buttonVal" value="over") Above (>)
                    md-radio(v-model="buttonVal" value="equal") Equals (=)
                    md-radio(v-model="buttonVal" value="under") Below (<)
            .input-field.md-size-30.md-layout-item
                md-field
                    label Amount (g)
                    md-input(v-model="amount" type="number")
            .confirm-buttons.md-size-10.md-layout-item
                md-button(:style="{'background-color' : 'green'}" @click="generateAndConfirm" :disabled="filter.confirmed").confirm-button.md-icon-button.md-raised
                    md-icon check
                md-button(:style="{'background-color' : 'darkred'}" :disabled="filter.confirmed === false" @click="deleteFilter").md-icon-button.md-raised.delete-button
                    md-icon delete


</template>

<script>
    export default {
        name: "FilterCard",

        props : {
            filter: Object
        },
        data() {
            return{
                nutrientChoice: Number,
                amount: Number,
                buttonVal: '',
                confirmButtonBackgroundColor: 'red'
            }
        },

        mounted(){
            console.log("this is my filter ID " + this.filter.key);
        },

        methods: {
            generateAndConfirm (){

                if(!this.confirmable){
                    console.log("not confirmable, error emitting!")
                    this.$emit('confirmError');
                }else{
                    this.filter.confirmed = true;
                    this.confirmButtonBackgroundColor = 'green';
                    this.$emit('confirm', this.filterArray);
                }

            },

            deleteFilter(){
                this.filter.deleted = true;
                this.$emit('delete', this.filter.key, this.filterArray);

            }
        },


        computed: {

            confirmable(){
                if(this.nutrientChoice && this.amount && this.buttonVal !== ''){
                    return true;
                }else{
                    return false;
                }
            },

             filterArray() {
                 let returnArray = [];
                 let returnObject = {};
                 returnObject[this.nutrientChoice] = this.amount;
                 returnArray.push(this.buttonVal);
                 returnArray.push(returnObject);
                 return returnArray
             }
        }
    }
</script>

<style scoped lang="stylus">

    .green-button
        background-color green

    .red-button
        background-color red

    .label-positioning
        position relative
        top 5px
        left 5px

    .buttons
        display flex
        flex-direction column
        align-items flex-start

    .confirm-buttons
        max-height 200px
        max-width 90px
        display flex
        flex-direction column
        align-items center

    .md-card
        max-height 500px

    .buttons
        align-items center
        justify-content center

    .confirm-button
        margin 5px
        background-color green

    .delete-button
        margin 5px
        background-color darkred



</style>