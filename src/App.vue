<template>
  <div>
    <div class="header">
      <h1>אתר הניהול של זלמי ושמואל</h1>
      <search @BrandSearch="BrandSearch" @VarietySearch="VarietySearch"></search>
      <AddProduct></AddProduct>
    </div>
 
    <div class="btns">
      <button @click="calculateTotalValue">שווי נוכחי של כל המוצרים יחד</button>
      <button @click="calculateTotalInvesting">סכום הכסף שהושקע</button>
      <button @click="calculateProfit">ממוצע רווחים שנתי</button>
    </div>
    <MyTable :items="prod"></MyTable>
  </div>
</template>

<script>
import axios from "axios";
import search from "./components/search.vue";
import AddProduct from "./components/Add-Product.vue";
import MyTable from "./components/Table.vue";
export default {
  components: { AddProduct, MyTable, search },
  name: "App",
  data() {
    return {
      bool: true,
      formValues: {
        ID: null,
        Brand: "",
        Variety: "",
        Year: null,
        Count: null,
        Date_purchase: "",
        Date_selling: "",
        Purchase_price: null,
        Over_all_purchase_price: null,
        Date_selling: "",
        Value: null,
        Over_all_value: null,
        Position: "",
        Date_purchase: "",
        Investor: "",
        Buyer_name: "",
        Profit: null,
        Seller_name: "",
        Selling_price: null,
      },
      items: [],
      activeIndex: -1,
    };
  },
  computed:{
    prod(){
      return this.items  
    }
  },
  mounted() {
    axios
      .get("https://salomon-wine-64bcf32bff86.herokuapp.com/api/products")
      .then((response) => {
        this.items = response.data;
      });
  },
  methods: {

    calculateTotalValue() {
      const t = this.items.reduce(
        (total, item) => total + item.Over_all_value,
        0
      );
      alert(t.toLocaleString());
    },

    calculateTotalInvesting() {
      const t = this.items.reduce(
        (total, item) => total + item.Over_all_purchase_price,
        0
      );
      const z = this.items.reduce((total, item) => {
        if (item.Investor == "זלמי") {
          return total + item.Over_all_purchase_price;
        } else {
          return total;
        }
      }, 0);
      const s = this.items.reduce((total, item) => {
        if (item.Investor == "שמואל") {
          return total + item.Over_all_purchase_price;
        } else {
          return total;
        }
      }, 0);

      alert(
        "סכום כולל שהושקע: " +
          t.toLocaleString() +
          ", " +
          "הסכום שהושקע ע''י זלמי: " +
          z.toLocaleString() +
          ", " +
          "הסכום שהושקע ע''י שמואל: " +
          s.toLocaleString()
      );
    },
    calculateProfit() {
      const t = this.items.reduce((total, item) => total + item.Profit, 0);
      const count = this.items.reduce(
        (total, item) => total + (item.Profit !== 0 ? 1 : 0),
        0
      );
      const y = t / count;
      alert(y);
    },
    BrandSearch(val) {
      axios
        .get(
          "https://salomon-wine-64bcf32bff86.herokuapp.com/api/products/brand/" +
            val
        )
        .then((response) => {
          this.items = response.data;
        });
    },
    VarietySearch(val) {
      axios
        .get(
          "https://salomon-wine-64bcf32bff86.herokuapp.com/api/products/variety/" +
            val
        )
        .then((response) => {
          this.items = response.data;
        });
    },
  },
};
</script>

<style scoped>
.btns{    display: flex;
    flex-direction: row;
    justify-content: center;
}
.form table {
  height: 67px;
}
h1{
  color: black;
}
.search {
  border-width: 1px;
  border-style: solid;
  padding: 5px;
  width: 19%;
  margin: auto;
}
h1{display: flex;
    justify-content: center;
  
    color: #8675e8;
}

button{
  box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
  display: inline-block;
                    outline: 0;
                    border: 0;
                    cursor: pointer;
                    background-color: #4299e1;
                    border-radius: 50px;
                    padding: 2px 10px;
    font-size: 12px;
                    font-weight: 700;
                    color: white;
                    line-height: 26px;
                
}
</style>
