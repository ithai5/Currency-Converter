<template>
  <div class="container">
    <InputHandler v-on:change-info="calculateFiat"/>
    <div id="result">
    {{rate}}
    </div>

  </div>
</template>

<script>
  import InputHandler from "@/components/InputHandler";
  import axios from "axios";
  export default {
    name: 'BodyContent',
    components: {
      InputHandler
    },
    data() {
      return {
        rate: null,
        faitUsd:null,
        cryptoUsd:null,
        apis:[ //can add more api following by fitting the api address with base and to currencies.
          ((base,to)=>'https://api.exchangeratesapi.io/latest?base='+base+'&symbols='+to),
          ((base,to)=>'https://api.binance.com/api/v3/ticker/price?symbol='+base+to),
        ],
        numberAfterDot:5
      }
    },
    methods: {
      async calculateFiat(info) {
        this.rate=''
        let num
        let res = null
        for (num = 0; num < this.apis.length && res===null;) {
          try {
            res = await axios.get(this.apis[num](info.cFrom, info.cTo))
            switch (num) { //matching the response to view, in case of adding more apis, need to add more cases
              case 0:
                this.rate = res.data['rates'][info.cTo] * info.amount
                break
              case 1:
                this.rate = res.data['price'] * info.amount
                break
              default:
                console.log('sorry but there is no api related')
            }
          }
          catch (err) {//if there is an error from the api we need to check next one
            console.log('error')
            num++
            res = null //reload the rate in a case that there is new recall
          }
        }
        if (res === null && num === this.apis.length) { //if we check the apis but there is no response we need to calculate between the two apis,
          this.crossAPIConvertor(info)
        }
      }
      ,
      crossAPIConvertor(info) { //should use USDC to calculate between the two apis
        axios
            .get('https://api.exchangeratesapi.io/latest?base=USD') //getting a list of all the fait currencies vs usd
            .then(res => {
              let fait = (res.data['rates'])
              axios
                  .get('https://api.binance.com/api/v3/ticker/price')//getting a list of all crypto currencies vs usdc
                  .then(res => {
                    let crypto
                    if(fait[info.cTo]!==undefined) {
                      fait = fait[info.cTo]
                      crypto = res.data.find(symbol=>symbol['symbol']===(info.cFrom + "USDC"))['price']
                      this.rate = crypto * fait * info.amount
                    }
                    else if(fait[info.cFrom]!==undefined){
                      fait = 1/fait[info.cFrom]
                      crypto = res.data.find(currency=>currency.symbol===(info.cTo + "USDC"))['price']
                      this.rate = fait / crypto * info.amount
                    }
                  }).catch((err) => console.log(err))
            }).catch((err) => console.log(err))}
    },
  }
</script>

<style scoped>

#result{
  position: fixed;
  width: 453px;
  height: 54px;
  left: 486px;
  top: 386px;


  background: #FFFFFF;
  border: 1px solid #D0D0D0;
  box-sizing: border-box;
  border-radius: 32px;


  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 24px;
  line-height: 28px;


  color: #939393;

}


</style>