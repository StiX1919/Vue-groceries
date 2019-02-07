<template>
    <v-card class='gCard' @mouseover="cardHover" @mouseout="cardHover">
        <div class='cartQuantity' v-if='cartQuantity'>
            <h3>{{cartQuantity}} in cart</h3>
        </div>
        <img class='gImg' :src='grocery.imageUrl' :alt='fakeImg'/>
        <div class='gDetails'>
            <h2>{{grocery.item}}</h2>
            <div class='hoverDetails'>
                <div class='cartQuantity' v-show="hovering">
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
        height: 200px;
        width: 250px;

        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;

        margin-bottom: 20px;
        margin-right: 5px;
        position: relative;
    }
    .gImg {
        width: 100%;
        height: 160px
    }
    .gDetails {
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 40px;
        padding-bottom: 10px;

        margin: 0 10px;
    }
    .hoverDetails {
        display: flex;
        height: 40px;
        justify-content: center;
        align-items: center
    }
    .addBox {
        display: flex;
        height: 40px;
        justify-content: space-around;
        align-items: center;

        position: absolute;
        bottom: 0;
        right: 50px;
        background-color: #424242;
        opacity: .9;
    }
    .numInput {
        width: 40px;
        height: 30px
    }

    .cartQuantity {
        position: absolute;

        right: 0px;
        bottom: 40px;
        color: white;
        background-color: #424242;
        height: 30px;
        width: 50%;
        display: flex;
        justify-content: center;
        align-items: center
    }


    @media only screen and (max-width: 753px) {
    .gCard {
      height: 300px;
      width: 330px;
    }
    .gImg {
        height: 250px
    }
    .gDetails {
        height: 50px
    }
    .cartQuantity {
        bottom: 50px
    }
  } 
</style>
