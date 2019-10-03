<template>
  <div id="app">
    <div class="row">
      <div class="col-md-2"></div>
      <div class="col-md-4">
        <input type="text" placeholder="Pesquisar produto" v-model="searchTerm" @keyup="searchProductsByTerm">
      </div>
      <div class="col-md-2"></div>
    </div>
    <div class="row">
      <div class="col-md-2">
        <Categories
          :categories="categories"
          :currentCategory="currentCategory"
          @setCurrentCategory="setCurrentCategory"
        />
      </div>
      <div class="col-md-5">
        <Result 
          :products="resultSearch"
          @insertProductInCart="insertProductInCart"
        />
      </div>
      <div class="col-md-3">
        <Cart
          :cartProducts='cartProducts'
          :totalSumCartProducts='totalSumCartProducts'
          @finalizeSale='finalizeSale'
          @removeItemCart='removeItemCart'
        />
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        
      </div>
    </div>
  </div>
</template>

<script>
import Categories from './components/Categories';
import Result from './components/Result';
import Cart from './components/Cart'
import axios from 'axios';

export default {
  name: 'app',
  components: {
    Categories,
    Result,
    Cart
  },
  data(){
    return{
      categories: [],
      products: [],
      resultSearch: [],
      cartProducts: [],
      totalSumCartProducts: 0,
      searchTerm: '',
      currentCategory: 0
    }
  },
  mounted(){
    let app = this;
    app.listCategories();
    app.listProducts();
  },
  methods:{
    async listCategories(){
      let app = this;
      try {
        let response = await axios.get('http://5d556e6936ad770014cce06d.mockapi.io/api/v1/catalog/products');
        app.categories = response.data.categories;
      } catch (error) {
        console.log(error);
      }
    },
    async listProducts(){
      let app = this;
      try {
        let response = await axios.get('http://5d556e6936ad770014cce06d.mockapi.io/api/v1/catalog/products');
        app.products = response.data.products;
        app.resultSearch = response.data.products;
      } catch (error) {
        console.log(error);
      }
    },
    async searchProductsByTerm(){
      let app = this;
      app.currentCategory = 0;
      app.resultSearch = [];
      for(var i = 0; i < app.products.length; i++){
        if(app.products[i].name.toLowerCase().match(app.searchTerm.toLowerCase())){
          app.resultSearch.push(app.products[i]);
        }
      }
    },
    async searchProductsByCategory(){
      let app = this;
      let category = app.currentCategory;
      app.resultSearch = [];
      for(var i = 0; i < app.products.length; i++){
        if(app.products[i].category.id == category){
          app.resultSearch.push(app.products[i]);
        }
      }
    },
    async insertProductInCart(data){
      let app = this;
      let product = data.id;
      //VERIFICA SE PRODUTO JÁ ESTÁ NO CARRINHO
      let cartProductsIsNull = 0;
      for(var j = 0; j < app.cartProducts.length; j++){
        if (app.cartProducts[j].id == product) {
          cartProductsIsNull = 1;
        }
      }

      if(cartProductsIsNull != 0){
        for(var k = 0; k < app.cartProducts.length; k++){
          if (app.cartProducts[k].id == product) {
            app.cartProducts[k].qnt = app.cartProducts[k].qnt + 1; 
          }
        }
      }else{
        for(var i = 0; i < app.products.length; i++){

          if(app.products[i].id == product){
            let data = {
              id: app.products[i].id,
              name: app.products[i].name,
              salePrice: app.products[i].salePrice,
              qnt: 1,
            }
            app.cartProducts.push(data);
          }
          
        }
      }

      app.sumCartProducts();

            
    },
    sumCartProducts(){
      let app = this;
      let total = 0;
      for(var i = 0; i < app.cartProducts.length; i++){
        total = total+(app.cartProducts[i].qnt*app.cartProducts[i].salePrice);
      }
      app.totalSumCartProducts = total;
    },
    setCurrentCategory(data){
      let app = this;
      app.currentCategory = data.id;
      app.searchProductsByCategory();
    },
    removeItemCart(data){
      let app = this;
      let id = data.id;

      for(var i = 0; i < app.cartProducts.length; i++){
        if (app.cartProducts[i].id == id) {
          if (app.cartProducts[i].qnt == 1) {
            app.cartProducts.splice(i);
          }else{
            app.cartProducts[i].qnt = app.cartProducts[i].qnt - 1;
          }
        }
      }

      app.sumCartProducts();

    },
    finalizeSale(){
      let app = this;
      app.totalSumCartProducts = 0;
      app.cartProducts = [];
      toastr.success('Venda Finalizada com Sucesso!');
    }
  }
}
</script>

<style scoped>
.row {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
}
.col-md-2{
  width: 20%;
}
.col-md-5{
  width: 50%;
}
.col-md-4{
  width: 40%;
}
.col-md-3{
  width: 30%;
}
.col-md-12{
  width: 100%;
}
input{
  width: 100%;
  height: 20px;
  font-size: 12pt;
}
</style>
