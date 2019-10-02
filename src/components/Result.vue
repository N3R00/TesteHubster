<template>
    <div class="result">
        <div class="row">
            <div class="col-md-2 product" v-for="p in products" :key="p.id" @click='insertProductInCart(p.id)'>
                <div class="row">
                    <div class="col-md-5">
                        <img :src="p.photo" alt="" class="imgProduct">
                    </div>
                    <div class="col-md-5">
                        <span class="title">{{p.name}}</span><br>
                        <span class="price"><b>R$ {{p.salePrice.toFixed(2).replace('.', ',')}}</b></span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Resultado',
    props: ['products'],
    methods:{
        insertProductInCart(id){
            this.$emit('insertProductInCart', {id: id});
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
  width: 25%;
  margin-left: 10px;
  margin-top: 15px;
  padding: 5px;
}
.col-md-8{
  width: 60%;
}
.col-md-5{
  width: 40%;
}
.col-md-4{
  width: 40%;
}
.col-md-12{
  width: 100%;
}
.result{
    margin-top: 10px;
}
.imgProduct{
    width: 50px;
}
.product{
    box-shadow: 1px 2px 3px #606060;
    cursor: pointer;
}
.title{
    font-size: 11pt;
}
</style>