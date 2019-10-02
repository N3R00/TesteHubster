<template>
    <div id="app">
        <div class="row">
            <ul>
                <li v-for="c in categories" :key='c.id'>
                  <div v-if="c.id == currentCategory">
                    <a href='' class="active" @click.prevent='setCurrentCategory(c.id)'>{{c.name}}</a>
                  </div>
                  <div v-else>
                    <a href='' @click.prevent='setCurrentCategory(c.id)'>{{c.name}}</a>
                  </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Categorias',
    props: ['categories', 'currentCategory'],
    methods:{
        setCurrentCategory(id){
            this.$emit('setCurrentCategory', {id: id});
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
.col-md-8{
  width: 60%;
}
.col-md-5{
  width: 50%;
}
.col-md-4{
  width: 40%;
}
.col-md-12{
  width: 100%;
}
ul{
    list-style: none;
    border: 1pt solid #606060;
}
li{
    padding: 3px;
}
li a{
    text-decoration: none;
    font-family: arial;
    color: #606060;
}
.active{
  color: #eee;
}
</style>