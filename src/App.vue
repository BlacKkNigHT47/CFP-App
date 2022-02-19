<template>
  <div id="app">
  <div>
    <div>
      <b-navbar toggleable="lg" type="dark" variant="primary">
      <b-navbar-brand href="#">QESCO</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
    </b-navbar>
    </div>
    <div>
      <b-table striped hover :items="items"></b-table>
    </div>
    <b-input v-model="inputText" v-on:keyup.enter="update"> </b-input>
    <b-button v-on:click="resetValue">clear</b-button>

  </div>
</div>

</template>

<script lang=ts>
import lookUpTable from './plugins/lookup-table'
export default {
  name: 'App',
  components: {
  },
                 
    
  
    data() {
      return {
        name: 'BootstrapVue',
        inputText:null,
        items:[]
      }
    },
    watch: {
    },
    methods: {
        update(){
        console.log(lookUpTable[0]);
        if (this.inputText != null) {
          var item = this.inputText;

          if (lookUpTable[item]) {
            var cost = parseFloat(lookUpTable[item].price).toFixed(2);
            var vat = (parseFloat(cost)*parseFloat(lookUpTable[item].VAT)*0.01).toFixed(2);
            var cfpvat= (parseFloat(cost) * 
                          (parseFloat(lookUpTable[item].CFPPrimary)*0.01 +
                          parseFloat(lookUpTable[item].CFPGlobal)*0.01 +
                          parseFloat(lookUpTable[item].CFPLocal)*0.01) ).toFixed(2);
            var total = parseFloat(cost) + 
                parseFloat(vat) + 
                parseFloat(cfpvat); 
            total = parseFloat(total.toFixed(2));
            console.log(cost, vat, cfpvat, total);
            
            
          this.items.push(
                {"Name":lookUpTable[item].name, 
                 "cost":cost, 
                 "Standard VAT":vat,
                 "CFP VAT": cfpvat,
                 "Total":total
                });
          } else {
                this.items.push(
                {"Name":"Unavailable", 
                 "cost":"Unavailable", 
                 "Standard VAT":"Unavailable",
                 "CFP VAT": "Unavailable",
                 "Total":"Unavailable"
                });
          }
           this.inputText=null;
          }
        },
        resetValue() {
          console.log("in the function - resetValue");
          this.items=[];
        }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
