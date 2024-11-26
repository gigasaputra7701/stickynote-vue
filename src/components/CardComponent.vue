<template>
  <transition-group name="fade" mode="out-in" appear>
    <div v-if="memos.length === 0" class="empty-memo">
      <p>Memo kosong...</p>
    </div>
    <div
      v-else
      v-for="memo in memos"
      class="card"
      :style="{ backgroundColor: memo.backgroundColor }"
      :key="memo.id"
    >
      <p class="card-content">
        {{ memo.content }}
      </p>
      <div class="card-footer">
        <p class="card-date">{{ memo.date }}</p>
        <button @click="onDelete(memo.id)" class="card-delete-btn">
          &times;
        </button>
      </div>
    </div>
  </transition-group>
</template>

<script setup>
defineProps({
  memos: Array,
  onDelete: Function,
});
</script>
<style scoped>
.card {
  width: 350px;
  height: 350px;
  padding: 10px;
  background-color: #ffa6c1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-sizing: border-box;
  transition: outline 0.3s ease;
}
.card:hover {
  outline: 1px solid rgba(0, 0, 0, 0.77); /* Outline saat hover */
}
.card-content {
  margin-top: 25px;
  font-size: 16px;
}
.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.card-date {
  font-size: 16px;
}
.card-delete-btn {
  position: absolute;
  top: 2px;
  right: 2px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  font-size: 25px;
  border: none;
  cursor: pointer;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease, transform 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
.empty-memo {
  position: absolute;
  top: 50%;
  color: #bb86fc;
  opacity: 0.8;
}
@media only screen and (min-width: 992px) {
  .card {
    width: 225px;
    height: 225px;
  }
}
</style>
