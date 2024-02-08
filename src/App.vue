<script setup>
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";
import AddTransaction from "./components/AddTransaction.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const total = computed(() => {
  return transactions.value.reduce((a, b) => a + b.amount, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((i) => i.amount > 0)
    .reduce((a, b) => a + b.amount, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((i) => i.amount < 0)
    .reduce((a, b) => a + b.amount, 0)
    .toFixed(2);
});

function generateUniqueId() {
  return Math.floor(Math.random() * 1_000_000);
}

function handleTransactionSubmitted(transactionData) {
  const id = generateUniqueId();
  transactions.value.push({ id, ...transactionData });

  saveToLocalStorage();
  toast.success("Transaction added");
}

function handleTransactionDeleted(id) {
  transactions.value = transactions.value.filter((i) => i.id !== id);

  saveToLocalStorage();
  toast.success("Transaction deleted");
}

function saveToLocalStorage() {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      @transaction-deleted="handleTransactionDeleted"
      :transactions="transactions"
    />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>
