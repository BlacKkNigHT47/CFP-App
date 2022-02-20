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
        <b-table striped hover :items="items" foot-clone=true>
            <template #foot(Total)>
                <div class="has-text-right">
                    {{outputText}}
                </div>
            </template>
            <template #foot()>
              <div></div>
            </template>
        </b-table>
      </div>
      <b-input id="main_input" v-model="inputText" v-on:keyup.enter="update"> </b-input>
      <b-button v-if="items.length" id="clear_button" v-on:click="resetValue">Clear</b-button>
    </div>
  </div>
</template>

<script lang=ts>
import lookUpTable from "./plugins/lookup-table";
export default {
  name: "App",
  components: {},

  data() {
    return {
      name: "BootstrapVue",
      inputText: null,
      items: [],
    };
  },
  watch: {},
  methods: {
    update() {
      if (this.inputText != null) {
        var gtin = this.inputText;

        if (lookUpTable[gtin]) {
          var cost = parseFloat(lookUpTable[gtin].price).toFixed(2);
          var vat = (
            parseFloat(cost) *
            parseFloat(lookUpTable[gtin].VAT) *
            0.01
          ).toFixed(2);
          var cfpvat = (
            parseFloat(cost) *
            (parseFloat(lookUpTable[gtin].CFPPrimary) * 0.01 +
              parseFloat(lookUpTable[gtin].CFPGlobal) * 0.01 +
              parseFloat(lookUpTable[gtin].CFPLocal) * 0.01)
          ).toFixed(2);
          var total = parseFloat(cost) + parseFloat(vat) + parseFloat(cfpvat);
          total = parseFloat(total.toFixed(2));
          console.log(cost, vat, cfpvat, total);

          let duplicateItem = this.items.find((item) => {
            return item.Name == lookUpTable[gtin].name;
          });
          if (duplicateItem) {
            console.log("Before filter: this.items.length", this.items.length);
            let items = this.items.filter(
              (item) => item.Name !== lookUpTable[gtin].name
            );
            console.log("After filter: this.items.length", items.length);
            this.items = items;
            duplicateItem.Quantity++;
            duplicateItem.Total = parseFloat(
              (duplicateItem.Total + total).toFixed(2)
            );
            this.items.push(duplicateItem);
          } else {
            this.items.push({
              Name: lookUpTable[gtin].name,
              Quantity: 1,
              cost: cost,
              "Standard VAT": vat,
              "CFP VAT": cfpvat,
              Total: total,
            });
          }
        } else {
          console.log("Barcode not found");
        }
        var totalCost = this.items
          .map((item) => item.Total)
          .reduce((acc, item) => item / 1 + acc / 1);
        this.inputText = null;
        this.outputText = `Total: ${parseFloat(totalCost).toFixed(2)}`;
      }
    },
    resetValue() {
      console.log("in the function - resetValue");
      this.items = [];
      this.outputText = "";
    },
  },
};
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
#display {
  background: #ffffff;
  font-family: 'Times New Roman', Times, sans-serif;
  text-align: right;
  font-size: 25px;
  
}
#clear_button {
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>
