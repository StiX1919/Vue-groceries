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
        <div class='cartQty'>
          <h4>{{cart.length}}</h4>
        </div>
        <v-img class='cartImg'  src='https://www.freeiconspng.com/uploads/cart-icon-16.png'/>
      </v-btn>
    </v-toolbar>

    <v-content >
      <div class='content'>

        <div class='sortBox'>
          <input class='sortInput' placeholder='Search for product' v-model='searchItem'/>
            <v-select 
                :items='categories' 
                placeholder="Sort by Category"
                hide-selected
                @change='sortByCat'
                height='30'
            />
        </div>
        <v-container>
          <div class='gCardHolder' label='Outline style'>
            <GroceryCard v-for='item in sortedItems' 
                        :key='item.item' 
                        :grocery='item' 
                        @addItem='addToCart' 
                        :cartQuantity='cartQuantity(item.item)'/>
          </div>
        </v-container>
      </div>
    </v-content>
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

      cart: [],
      sortCat: '',
      searchItem: ''
    }
  },
  mounted() {
      axios.get('/api/getList').then(response => {
        this.groceries = response.data.list
        
    }).catch(err => console.log(err, 'error'))
    
  },
  methods: {
    addToCart: function(item) {
        this.clipped = false
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


    },
    openCart: function(){
      this.clipped = !this.clipped
    },

    sortByCat: function(e){
      this.clipped = false
      this.searchItem = ''
      this.sortCat = e
    },
    searchInput: function(e){
      this.clipped = false
      
      this.searchItem = e.target.value
    },

    cartQuantity: function(name){
      let quantity = 0
      if(this.cart[0]){
        let item = this.cart.find(item => {
          return item.grocery.item === name
        })
        
        if(item !== undefined){
          quantity = item.quantity
        }
      }
      
      return quantity
    }
  },
  computed: {
    categories: function(){
      let list = ['']

      this.groceries.map(item => {
        if(!list.includes(item.category)){
          list.push(item.category)
        }
      })
      
      return list
    },
    sortedItems: function(){
      let categories = this.groceries.filter((item, index) => {
        return item.category.toLowerCase().includes(this.sortCat.toLowerCase())
      })

      return categories.filter((item, index) => {
        return item.item.toLowerCase().includes(this.searchItem.toLowerCase())
      })
    }
  },


}
</script>

<style>
  .content {
    display: flex
  }

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
  .sortBox {
    height: 500px;
    width: 300px
  }
  .sortInput {
    margin-top: 30px;
    height: 32px;

    border-bottom: 20px; 
  }
  
  .cartQty {
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;

    border-radius: 100%;
    background: goldenrod;
    color: white;
    width: 55%;
    margin-left: 10px
  }
  @media only screen and (max-width: 731px) {
    .sortBox {
      width: 100%
    }
  } 

  @media only screen and (max-width: 600px) {
    .sortBox {
      display: block;
      height: 100px
    }
    .sortInput {
      height: 50px;
      width: 100%
    }
  }
</style>
