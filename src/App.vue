<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Sticky Notes</h1>
        <Button @click="openForm" class="header-button">+</Button>
      </header>
      <div class="card-container">
        <CardComponent :memos="memos" :onDelete="deleteMemo" />
      </div>
    </div>
    <FormComponent
      :showForm="showForm"
      :errorMessage="errorMessage"
      @update:showForm="(val) => (showForm = val)"
      @addMemo="addMemo"
    />
  </main>
</template>

<script setup>
import { ref } from "vue";
import CardComponent from "./components/CardComponent.vue";
import FormComponent from "./components/FormComponent.vue";

const showForm = ref(false);
const memos = ref([]);
const errorMessage = ref("");

const openForm = () => {
  showForm.value = true;
  errorMessage.value = "";
};

const addMemo = (content) => {
  if (!content.trim()) {
    errorMessage.value = "Please enter a memo!";
    return;
  }
  memos.value.push({
    id: Date.now(),
    content,
    date: new Date().toLocaleDateString("en-us"),
    backgroundColor: getRandomColor(),
  });
  showForm.value = false; // Tutup modal setelah menyimpan memo
};

const deleteMemo = (id) => {
  memos.value = memos.value.filter((memo) => memo.id !== id);
};

const getRandomColor = () => {
  const hue = Math.floor(Math.random() * 360);
  return `hsl(${hue}, 100%, 80%)`;
};
</script>
<style scoped>
main {
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100vw;
}
.container {
  max-width: 900px;
  padding: 12px;
  margin: 0 auto;
}
header {
  position: relative;
  width: 100wh;
  display: flex;
  justify-content: center;
}
.header-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: rgba(255, 255, 255, 0.8);
  text-align: center;
}
.header-button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  background-color: #bb86fc;
  color: #121212;
  position: fixed;
  bottom: 10px;
  font-size: 16px;
  z-index: 9;
  transition: all 300ms ease-in-out;
}
.header-button:hover {
  transition: all 300ms ease-in-out;
  opacity: 0.8;
}
.card-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 20px;
  z-index: -1;
}
</style>
