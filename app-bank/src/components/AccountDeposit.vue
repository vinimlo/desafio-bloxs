<template>
  <div class="module-container">
    <h3>Depósito</h3>
    <form @submit.prevent="deposit">
      <label for="amount">Valor</label>
      <input type="number" name="amount" id="amount" v-model="amount" :disabled="isAccountLocked" placeholder="0.00" required min="0" step="0.01" pattern="^\d+(?:\.\d{1,2})?$">

      <input type="submit" value="Depositar" :disabled="isAccountLocked">
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'AccountDeposit',
  emits: ['updateBalance'],
  data() {
    return {
      accountId: 0,
      amount: 0
    }
  },
  props: {
    isAccountLocked: {
      type: Boolean,
      default: false
    }
  },
  mounted() {
    this.accountId = JSON.parse(
      String(localStorage.getItem('user_account'))
    ).id;
  },
  methods: {
    deposit() {
      let amount = this.amount;
      let account_id = this.accountId;
      axios.post('http://localhost:5000/deposit', {
        amount: amount,
        account_id: account_id
      })
        .then((response) => {
          this.$emit('updateBalance');
        })
        .catch(function (error) {
          console.log(error);
        });
    }
  }
});
</script>

<style scoped>
label {
  padding-top: 0;
}
</style>