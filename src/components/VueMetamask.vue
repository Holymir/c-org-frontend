<template>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-sm-offset-3 col-md-6 col-md-offset-3">
        <vue-metamask @onComplete="onComplete"></vue-metamask>
        <button @click="getBalance">GetBalance</button>
        <p>ETH Balance: {{ balanceEth | toHRFormat }}</p>
        <p>MGL Balance: {{ HRBalance }}</p>
      </div>
    </div>
</template>

<script>
  import VueMetamask from 'vue-metamask';
  require("regenerator-runtime/runtime");

  const DAI = 1000000000000000000; // 1 DAI

  export default {
    components: {
      VueMetamask,
    },
    data(){
      return {
        data: Object,
        balanceEth: 0,
        balanceMgl: 0
      }
    },
    methods:{
      onComplete(data) {
        this.data = data;
        this.getBalance();
      },
      getBalance() {
        this.data.web3.eth.getBalance(this.data.metaMaskAddress, (err, res) => {
          if(err) {
            console.log("err", err);
          }
          this.balanceEth = res;
          console.log("res", res.toString());
        })
      }
    },
    filters: {
      toHRFormat(value) {
        return (value / DAI).toFixed(3);
      }
    },
    computed: {
      /**
       * @return {number}
       */
      HRBalance() {
        return (this.balanceEth / DAI).toFixed(3);
      }
    }
  }
</script>

<style>

</style>
