<template>
    <v-navigation-drawer
      v-model='clipped'
      fixed
      temporary
      app
      right
      width='500'
      class='cart'
      height='100vh'
    >
      <v-toolbar sticky>
        <v-toolbar-title>Current Cart</v-toolbar-title>
        <v-spacer />
        <v-btn color='black' icon small @click='openCart'>X</v-btn>
      </v-toolbar>

      <div class='detailHeader'>
          <h3>Item</h3>
          <h3>Quantity</h3>
          <h3>Price</h3>
      </div>
      <div class='itemHolder'>

        <CartItem v-for='(item, index) in cart' 
                :key='index' 
                :index='index'
                :item='item' 
                :cart='cart'
                @removeItem='removeItem'/>

      </div>

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
                    
                    return acc + (item.grocery.price * item.quantity)
                }, 0)
            }
            
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
      width: 80%;
      display: flex;
      justify-content: space-around;

      margin-top: 20px
  }

    .itemHolder {
        margin: 10px 0;
        height: 70%;
        overflow: scroll
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
