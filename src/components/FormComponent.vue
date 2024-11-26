<template>
  <div>
    <transition name="fade">
      <div v-if="showForm" class="form-overlay">
        <div class="form-modal">
          <span v-if="errorMessage" class="error-message">{{
            errorMessage
          }}</span>
          <button @click="closeForm" class="form-close-btn">&times;</button>
          <textarea v-model="localMemo" cols="30" rows="10"></textarea>
          <button @click="handleAdd" class="form-save-btn">Save</button>
        </div>
      </div>
    </transition>
    <transition name="slide" class="success-popup">
      <div v-if="showSuccess" class="success-message">
        Memo successfully added!
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref } from "vue";

defineProps({
  showForm: Boolean,
  errorMessage: String,
});

const showSuccess = ref(false);
const emit = defineEmits(["update:showForm", "addMemo"]);
const localMemo = ref("");

const handleAdd = () => {
  emit("addMemo", localMemo.value);
  localMemo.value = "";
  showSuccess.value = true;

  setTimeout(() => {
    showSuccess.value = false;
  }, 2000);
};

const closeForm = () => {
  emit("update:showForm", false);
};
</script>

<style scoped>
textarea {
  font-size: 16px;
}
.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.77);
  backdrop-filter: blur(6px);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.form-modal {
  width: 300px;
  background-color: #1f1b24;
  border-radius: 10px;
  padding: 45px 15px 15px 15px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.form-save-btn {
  padding: 10px 20px;
  font-size: 16px;
  width: 100%;
  background-color: #bb86fc;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 15px;
  color: #121212;
}
.form-close-btn {
  position: absolute;
  top: 2px;
  right: 2px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  font-size: 25px;
  border: none;
  cursor: pointer;
  color: #ffffff;
}
.error-message {
  color: red;
  margin-bottom: 5px;
}
.success-popup {
  position: absolute;
}
.success-message {
  position: fixed;
  top: 20px;
  right: 0;
  /* transform: translateX(-50%); */
  background-color: #03dac6;
  color: #121212;
  padding: 15px 30px;
  border-radius: 5px 0px 0px 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  z-index: 1000;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s ease, opacity 0.5s ease;
}
.slide-enter-from,
.slide-leave-to {
  transform: translateX(30px);
  opacity: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
