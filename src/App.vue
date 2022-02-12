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
 /*       
            var lookUpTable = {
              "8003340091471": {
                                  "name": "Lindt Sprungli chocolate",
                                  "country": "Switzerland",
                                  "price": 6.00,
                                  "CFPPrimary": 2.00,
                                  "CFPGlobal": 10.00,
                                  "CFPLocal":2.00,
                                  "VAT":20.00
                               },
              "8906021122108": {
                   
                                  "name": "Achi's Cut Mango Pickle",
                                  "country": "India",
                                  "price": 2.49,
                                  "CFPPrimary": 0.00,
                                  "CFPGlobal": 20.00,
                                  "CFPLocal":2.00,
                                  "VAT":20.00
                               }
             };
*/
          console.log(lookUpTable[0]);
          if (this.inputText != null) {
            var item = this.inputText;

            if (lookUpTable[item]) {
              var cost = parseFloat(lookUpTable[item].price).toFixed(2);
              var vat = (cost*parseFloat(lookUpTable[item].VAT)*0.01).toFixed(2);
              var cfpvat= (cost * 
                           (parseFloat(lookUpTable[item].CFPPrimary)*0.01 +
                            parseFloat(lookUpTable[item].CFPGlobal)*0.01 +
                            parseFloat(lookUpTable[item].CFPLocal)*0.01) ).toFixed(2);
              var total = parseFloat(cost) + 
                  parseFloat(vat) + 
                  parseFloat(cfpvat); 
              total = total.toFixed(2);
              console.log(cost, vat, cfpvat, total);
            
            
    this.items.push(
                {"Name":lookUpTable[item].name, 
                 "cost":cost, 
                 "Standard VAT":vat,
                 "CFP VAT": cfpvat,
                 "Total":total
                });
          }
           this.inputText=null;
          }
        
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
