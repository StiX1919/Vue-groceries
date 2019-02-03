<template>
    <v-navigation-drawer
      v-model='clipped'
      fixed
      temporary
      app
      right
      width='600'
      class='cart'
      height='100vh'
    >
      <v-toolbar>
        <v-toolbar-title>Current Cart</v-toolbar-title>
        <v-spacer />
        <v-btn color='black' icon small @click='openCart'>X</v-btn>
      </v-toolbar>

      <div class='detailHeader'>
          <h3>Item</h3>
          <h3>Quantity</h3>
          <h3>Price</h3>
      </div>
      <v-divider />

        <CartItem v-for='(item, index) in cart' 
                :key='index' 
                :index='index'
                :item='item' 
                :cart='cart'
                :removeItem='removeItem'/>
        <div class='footer'>
            <v-divider />
            <h1>Total: {{cartTotal}}</h1>
        </div>
            
    </v-navigation-drawer>

</template>

<script>
import CartItem from './cartItem'

export default {
    name: 'CartComponent',
    components: {
        CartItem
    },
    props: {
        clipped: Boolean,
        cart: Array,
        openCart: Function
    },
  data() {
    return {
    }
  },
  methods: {
      removeItem: function(index) {
          this.cart.splice(index, 1)
      }
  },
  computed: {
      cartTotal: function(){

          let total = 0
            if(this.cart[0]){
                total = this.cart.reduce((acc, item) => {
                    console.log(acc, 'in reduce', item.grocery.price * item.quantity)
                    return acc + (item.grocery.price * item.quantity)
                }, 0)
            }
            console.log('out of reduce', total)
          return `$${total.toFixed(2)}`
      }
  }

}
</script>

<style>
    .cart {
        display: flex;
        flex-direction: column;
        height: 100vh
    }
    .detailHeader {
      width: 60%;
      display: flex;
      justify-content: space-around;

      margin-top: 20px
  }
  .footer{
      width: 100%;
      height: 50px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center

  }
  
</style>
