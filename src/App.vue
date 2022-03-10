<template>
  <div id="app">
    <form @submit.prevent="submit">
      <div class="form-control">
        <label class="form-label" for="email">Email Address</label>
        <input id="email" class="form-input" type="email" v-model="email" placeholder="email adddress" />
      </div>
      <div class="form-control">
        <label class="form-label" for="fullname">Full name</label>
        <input id="fullname" class="form-input" type="text" v-model="fullname" placeholder="john doe" />
      </div>
      <div class="form-control">
        <label class="form-label" for="amount">Amount (NGN)</label>
        <input id="amount" class="form-input" type="number" min="0" step="100" v-model="amount" placeholder="1000" />
      </div>
      <button class="form-button" type="submit">Click to Pay {{ amount }}</button>
    </form>
    <pre v-if="hasPayload">
      {{ payload }}
    </pre>
  </div>
</template>

<script>
import FluidcoinsPay from '@fluidcoins/pay.js'

export default {
  name: 'App',
  components: {},
  data() {
    return {
      amount: 0,
      fullname: '',
      email: '',
      payload: {}
    }
  },
  computed: {
    hasPayload() {
      return Object.keys(this.payload).length > 0 
    }
  },
  methods: {
    submit() {
      const  { amount, fullname, email, onSuccess} = this

      const config = {
        key: 'pk_test_e079029d17c14d07a044e1fdc0434e64',
        amount: amount * 100,
        email: email,
        name: fullname,
        onSuccess,
      }
      
      try {
        const fPay = new FluidcoinsPay(config)
        fPay.setup() // sets up the widget
        fPay.open() // opens the widget
      } catch (error) {
        console.log(error) // log any error from Fluidcoins Pay validations
      }
    },
    onSuccess(payload){
      this.payload = payload
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

.form-control {
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
}

form {
  max-width: 250px;
  width: 100%;
  margin: 0 auto;
}
.form-button {
  background: indigo;
  font-size: 1rem;
  padding: 12px 16px;
  color: white;
  border: none;
  border-radius: 8px;
}

.form-input {
  padding: 12px 8px;
  font-size: 1rem;
  border-radius: 8px;
  border: 0.5px solid brown;
  margin-top: 6px;
}

.form-label {
  text-align: left;
}

</style>
