<template>
        <div>

            <div class="itemHeader" >
                <h2>{{item.grocery.item}}</h2>
                <input type='number' 
                            :value='item.quantity' 
                            @change="changeAmount"
                            class='itemQuantity'
                    />
                <h2>{{truePrice}}</h2>

                <div >
                    
                </div>
                <v-btn color='red' icon small @click='removeItem(index)'>X</v-btn>
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

</style>
