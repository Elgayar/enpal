<!-- the root component of our app -->

<template>

 <div id="app">
<Navigationbar @select-quantity="chosenQuantity=$event"/> 
<Convert v-bind:physicalQuantity="chosenQuantity"/> 
  </div> 

</template>


<!-- the logic of this component: -->
<script>
import Navigationbar from './components/Navigationbar.vue'            /* import some other components */
import Convert from './components/Convert.vue'




export default {
  name: 'app',
  components: {  /* list all other components which we use inside this component */
    Convert,
    Navigationbar,
  },
  data: function(){
    return{
      chosenQuantity: {name:"Family",id:0, units:["kWh","Person"]} 
     /* the currently chosen quantity. Per default, the chosen quantity is kWh*/
    }
  },
  methods:{
    /* ... */
  },
  computed: {
    getMeasure(){
      return this.quantities.find(x => x.name==this.chosenQuantity);
    }
  }
}
</script>


<!-- some CSS styling. There is no "scoped" attribute, so this styling applies to the whole app -->
<style>
  body{
    margin:0;
    font-family:"Source Sans Pro";
    background-image: url('./assets/panel3.jpg');
    background-size: cover ;
  }
 
  #app{
    display:flex;
  }
</style>
