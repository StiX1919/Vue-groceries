<template>
    <v-card class='gCard' @mouseover="cardHover" @mouseout="cardHover">
        <div class='cartQuantity' v-if='cartQuantity'>
            <h3>{{cartQuantity}} in cart</h3>
        </div>
        <img class='gImg' :src='grocery.imageUrl' :alt='fakeImg'/>
        <div class='gDetails'>
            <h2>{{grocery.item}}</h2>
            <div class='gDetails'>
                <div class='gDetails' v-show="hovering">
                    <input type='number' 
                            class='numInput'
                            
                            placeholder="Input Qty" 
                            v-model='quantity' 
                            @change="updateAmount"
                            />
                    <v-btn color='green' icon small :disabled="quantity < 1" @click='addToCart({grocery, quantity})'>+</v-btn>
                </div>
                <h4>{{ truePrice }}</h4>

            </div>
        </div>
    </v-card>
</template>

<script>

export default {
    name: 'GroceryCard',
    props: {
        grocery: {
            item: String,
            category: String,
            price: Number,
            imageUrl: String,
            
        },
        addItem: Function,
        cartQuantity: Number
    },
    data() {
        return {
            price: 0,
            fakeImg: 'http://www.independentmediators.co.uk/wp-content/uploads/2016/02/placeholder-image.jpg',
            hovering: false,
            quantity: 1,
        }
    },
    methods: {
        cardHover: function() {
            this.hovering = !this.hovering
        },
        updateAmount: function(e){
            this.quantity = e.target.value
        },
        addToCart: function(obj) {
            this.$emit('addItem', obj)
            this.quantity = 1
        }
    },
    computed: {
        truePrice: function(){
            let newPrice = `$${this.grocery.price.toFixed(2)}`

            return newPrice
        },
        placeHolderImg: function(){
            return `Picture of ${this.grocery.item}`
        },
        
    }

  
}
</script>

<style >
    .addButton {
        width: 20px;
        height: 20px;
    }

    .gCard {
        height: 300px;
        width: 350px;

        display: flex;
        flex-direction: column;
        justify-content: space-between;

        margin-bottom: 20px;
        position: relative;
    }
    .gImg {
        width: 100%;
        height: 80%
    }
    .gDetails {
        display: flex;
        justify-content: space-around;
        align-items: center
    }
    .numInput {
        width: 40px;
        height: 30px
    }

    .cartQuantity {
        position: absolute;

        right: 0px;
        bottom: 60px;
        color: white;
        background-color: #424242;
        height: 30px;
        width: 50%;
        display: flex;
        justify-content: center;
        align-items: center
    }
</style>
