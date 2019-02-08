<template>
        <div>

            <div class="itemHeader" >
                <h2 class='itemName'>{{item.grocery.item}}</h2>
                <input type='number' 
                            v-model='item.quantity' 
                            @change="changeAmount"
                            class='itemQuantity'
                    />
                <h2 class='truePrice'>{{truePrice}}</h2>

                
                <v-btn color='red' icon small @click='deleteItem(index)'>X</v-btn>
            </div>
            <v-divider v-if='index + 1 < cart.length'/>
        </div>

</template>

<script>

export default {
    name: 'CartItem',
    props: {
        item: Object,
        index: Number,
        cart: Array,
        removeItem: Function
    },
  data() {
    return {
    }
  },
  methods: {
      changeAmount: function(e){
          this.cart[this.index].quantity = e.target.value
      },
      deleteItem(index){
          this.$emit('removeItem', index)
      }
  },
  computed: {
      truePrice: function(){
            let newPrice = `$${(this.item.grocery.price * this.item.quantity).toFixed(2)}`

            return newPrice
        }
  }

}
</script>

<style>
  .itemHeader {
      width: 100%;
      display: flex;
      justify-content: space-around;
      align-items: center;

      margin-top: 20px
  }
  .itemQuantity {
      width: 50px
  }

  .itemName {
      width: 30%
  }
  .truePrice {
      width: 20%
  }

</style>
