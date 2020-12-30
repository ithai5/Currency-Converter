<template>

  <div>
    <br><br><br>
    <form @submit="collectInfo" class="collect-info">
      <div class="collect-input">
        <label>
          <input type="number" v-model="amount" placeholder="amount" class="inputBox" required>
        </label>
        <label>
          <input list="currenciesFrom" v-model="cFrom" placeholder="Currency" class="inputBox" required>
          <datalist id="currenciesFrom" >
            <option v-for="currency in currencies" :value="currency" v-bind:key="currency">{{currency}}</option>
          </datalist>
        </label>
        <label>
          <input list="currenciesTo" v-model="cTo" placeholder="To Currency" class="inputBox" required>
          <datalist id="currenciesTo">
            <option v-for="currency in currencies" :value="currency" v-bind:key="currency">{{currency}}</option>
          </datalist>
        </label>
        <br><br>
        <input type="submit" value="Calculate" class="btn">

      </div>
    </form>
  </div>
</template>

<script>

import axios from "axios";

export default {
  name: "InputHandler",
  data() {
    return {
      cFrom: '',
      cTo: '',
      amount: '',
      currencies: [],
    }
  },
  methods: {
    collectInfo(e) {
      e.preventDefault()
      const info = {
        cFrom: this.cFrom.toUpperCase(),
        cTo: this.cTo.toUpperCase(),
        amount: this.amount
      }
      //send up to the parent
      this.$emit('change-info', info)

    }
  },

  created(){ //method to collect all currencies
        axios
            .get('https://api.binance.com/api/v3/ticker/price')//getting a list of all crypto currencies vs usdc
            .then(res => {
              res.data.map(currency => {
                const regex = /.+USDC/
                if(regex.test(currency.symbol)){
                  this.currencies.push(currency.symbol.replace('USDC',''))
                  this.currencies.sort()
                }
              })
            })
            .catch(err => console.log(err))
    axios
        .get('https://api.exchangeratesapi.io/latest?base=USD')//getting a list of all fait currencies
        .then(res => {
          const faitCurrencies = Object.keys(res.data['rates'])
          faitCurrencies.map(currency=>this.currencies.push(currency))
          this.currencies.sort()
        })
        .catch(err => console.log(err))
  },
}
</script>

<style scoped>
.btn{
  cursor: pointer;
  width: 166px;
  height: 48px;
  top: 287px;
  font-family: Roboto,serif;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  line-height: 14px;
  /* identical to box height */


  color: #FFFFFF;


  background: #599210;
  border: 1px solid #599210;
  box-sizing: border-box;
  border-radius: 40px;

}
.collect-input{
  position: fixed;
  height: 38px;
  top: 198px;
  display: contents;
}







</style>