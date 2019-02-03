<template>
  <v-app dark>
    <v-toolbar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn icon @click="clipped = !clipped">
        <v-img class='cartImg'  src='https://www.freeiconspng.com/uploads/cart-icon-16.png'/>
      </v-btn>
    </v-toolbar>

    <v-content >
      <v-container>
        <div class='gCardHolder'>
          <GroceryCard v-for='item in groceries' :key='item.item' :grocery='item' :addItem='addToCart'/>
        </div>
      </v-container>
    </v-content>
  
    <v-navigation-drawer
      v-model='clipped'
      absolute
      temporary
      app
      right
      width='600'
    >
      <v-toolbar>
        <v-toolbar-title>Current Cart</v-toolbar-title>
        <v-spacer />
        <v-btn color='black' icon small @click='clipped = !clipped'>X</v-btn>
      </v-toolbar>

      

    </v-navigation-drawer>
    <cart-component :openCart='openCart' :clipped='clipped' :cart='cart'/>
    
  </v-app>
</template>

<script>
import axios from 'axios'
import GroceryCard from '../components/groceryCard'
import CartComponent from '../components/cart'

export default {
  components: {
    GroceryCard,
    CartComponent
  },
  data() {
    return {
      clipped: false,
      fixed: false,
      groceries: [],
      items: [
        {
          icon: 'apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'bubble_chart',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      title: `Spencer's Grocery`,

      cart: []
    }
  },
  mounted() {
      axios.get('/api/getList').then(response => {
        this.groceries = response.data.list
        console.log(this.groceries)
    }).catch(err => console.log(err, 'error'))
    
  },
  methods: {
    addToCart: function(item) {
      let inside = false

      this.cart.map((cartItem, index) => {
        if(item.grocery.item === cartItem.grocery.item) {
          inside = true;

          this.cart[index].quantity = +item.quantity + +this.cart[index].quantity
        }
      })

      if(inside === false){
        this.cart.push(item)
      }

      console.log(this.cart)
    },
    openCart: function(){
      this.clipped = !this.clipped
    }
  }

}
</script>

<style>
  .gCardHolder {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;

    width: 100%;

  }
  .cartImg {
    /* height: 20px;
    width: 20px; */

   margin-left: 3px
  }
  

</style>
