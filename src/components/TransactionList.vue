<script setup>
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(["transactionDeleted", "transactionsCleared"]);

function deleteTransaction(id) {
  emit("transactionDeleted", id);
}

function clearTransactions() {
  emit("transactionsCleared");
}
</script>

<template>
  <h3>History</h3>
  <ul class="list">
    <li
      v-for="transaction in transactions"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
      :key="transaction.id"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button @click="deleteTransaction(transaction.id)" class="delete-btn">
        x
      </button>
    </li>
  </ul>
  <button v-if="transactions.length" @click="clearTransactions" class="btn">
    Clear list
  </button>
</template>

<style scoped>
.list {
  list-style-type: none;
  padding: 0;
  margin-bottom: 40px;
}

.list li {
  background-color: #fff;
  box-shadow: var(--box-shadow);
  color: #333;
  display: flex;
  justify-content: space-between;
  position: relative;
  padding: 10px;
  margin: 10px 0;
}

.list li.plus {
  border-right: 5px solid #2ecc71;
}

.list li.minus {
  border-right: 5px solid #c0392b;
}

.delete-btn {
  cursor: pointer;
  background-color: #e74c3c;
  border: 0;
  color: #fff;
  font-size: 20px;
  line-height: 20px;
  padding: 2px 5px;
  position: absolute;
  top: 50%;
  left: 0;
  transform: translate(-100%, -50%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.list li:hover .delete-btn {
  opacity: 1;
}

.delete-btn:focus {
  outline: 0;
}

.btn {
  cursor: pointer;
  background-color: #65e4f5;
  box-shadow: var(--box-shadow);
  color: #000;
  border: 0;
  display: block;
  font-size: 16px;
  margin: 10px 0 30px;
  padding: 10px;
  width: 100%;
}
</style>
