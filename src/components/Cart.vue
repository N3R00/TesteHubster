<template>
    <div class="cart">
        <div class="row">
            <div class="col-md-12 product" v-for="p in cartProducts" :key='p.id'>
                <div class="row">
                    <div class="col-md-6">
                        <span>{{p.qnt}} x {{p.name}}</span>
                    </div>
                    <div class="col-md-4">
                        <span>R$ {{(p.salePrice*p.qnt).toFixed(2).replace('.', ',')}}</span>
                    </div>
                </div>
                <span><small>Valor Unitário: </small>R$ {{p.salePrice.toFixed(2).replace('.', ',')}}</span>
            </div>
        </div>
        <div class="row">
            <div class="col-md-5">Subtotal</div>
            <div class="col-md-5">R$ {{totalSumCartProducts.toFixed(2).replace('.', ',')}}</div>
            <hr>
        </div>
        <div class="row">
            <div class="col-md-5">Desconto</div>
            <div class="col-md-5"><input type="number" placeholder="%" v-model="porcentage" @keyup="setDiscount"></div>
            <hr>
        </div>
        <div class="row">
            <div class="col-md-5">Subtotal</div>
            <div class="col-md-5">R$ {{total.toFixed(2).replace('.', ',')}}</div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <button @click='finalizeSale'>Finalizar Venda</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Carrinho',
    props: ['cartProducts', 'totalSumCartProducts'],
    data(){
        return{
            total: 0,
            porcentage: 0
        }
    },
    methods:{
        setDiscount(){
            let app = this;
            let porcentage = app.porcentage / 100;
            let valueDiscount = app.totalSumCartProducts * porcentage;
            app.total = app.totalSumCartProducts - valueDiscount;
        },
        finalizeSale(){
            this.$emit('finalizeSale');
        }
    }, 
    watch: {
        totalSumCartProducts: function () {
            this.total = this.totalSumCartProducts;
        }
    },
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
.col-md-8{
  width: 60%;
}
.col-md-5{
  width: 50%;
}
.col-md-6{
  width: 60%;
}
.col-md-4{
  width: 40%;
}
.col-md-12{
  width: 100%;
}
input{
  width: 50%;
  height: 20px;
  font-size: 12pt;
}
.product{
    margin-top: 5px;
    margin-bottom: 5px;
    border: 1pt solid #606060;
    padding: 5px;
}
.cart{
    box-shadow: 1px 2px 3px #eee;
    padding: 10px;
}
button{
    width: 100%;
    height: 30px;
    background-color: #fff;
    border: 1pt solid rgb(202, 200, 200);
    font-size: 12pt;
    color: #333;
}
</style>