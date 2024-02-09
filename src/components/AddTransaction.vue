<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const text = ref("");
const amount = ref("");

const emit = defineEmits(["transactionSubmitted"]);

function onSubmit() {
  if (!text.value || !amount.value) {
    toast.error("Both fields must be filled");
    return;
  }

  if (isNaN(amount.value)) {
    toast.error("Amount must be a number");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: Number(parseFloat(amount.value).toFixed(2)),
  };

  emit("transactionSubmitted", transactionData);

  text.value = "";
  amount.value = "";
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input v-model="amount" type="text" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<style scoped>
label {
  display: inline-block;
  margin: 10px 0;
}

input[type="text"],
input[type="number"] {
  border: 1px solid #dedede;
  border-radius: 2px;
  display: block;
  font-size: 16px;
  padding: 10px;
  width: 100%;
}

.btn {
  cursor: pointer;
  background-color: #9c88ff;
  box-shadow: var(--box-shadow);
  color: #fff;
  border: 0;
  display: block;
  font-size: 16px;
  margin: 10px 0 30px;
  padding: 10px;
  width: 100%;
}

.btn:focus {
  outline: 0;
}
</style>
