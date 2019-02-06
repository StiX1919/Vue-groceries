<template>
    <v-card class='addCard' 
            @mouseover='hoverFunc'
            @mouseout='hoverFunc'
            :color="hovering ? '#303030' : '#424242'"
            >
        <h3>Not finding what you're looking for?</h3>

        <h4>Add new items here!!</h4>
    </v-card>
</template>

<script>
import axios from 'axios'

export default {
    name: 'AddItem',
    data: function(){
        return {
            hovering: false
        }
    },
    methods: {
        hoverFunc: function(){
            this.hovering = !this.hovering
        }
    }
  
}
</script>

<style >
  .addCard {
        height: 200px;
        width: 250px;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;

        padding: 10px;

        margin-bottom: 20px;
        margin-right: 5px;
        cursor: pointer;
    }
    
</style>
