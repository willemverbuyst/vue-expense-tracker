<script setup>
import { computed, ref } from "vue";
import { useToast } from "vue-toastification";
import AddTransaction from "./components/AddTransaction.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";

const toast = useToast();

const transactions = ref([
  {
    id: 1,
    text: "Flower",
    amount: -19.99,
  },
  {
    id: 2,
    text: "Salary",
    amount: 299.97,
  },
  {
    id: 3,
    text: "Book",
    amount: -10,
  },
  {
    id: 4,
    text: "Camera",
    amount: 150,
  },
]);

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

  toast.success("Transaction added");
}

function handleTransactionDeleted(id) {
  transactions.value = transactions.value.filter((i) => i.id !== id);

  toast.success("Transaction deleted");
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
