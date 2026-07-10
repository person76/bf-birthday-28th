<template>
  <div class="pink-full-screen" :class="{ 'fade-out': isFading }">
    <div class="fake-birthday-box">
      <span class="heart-icon">❤️</span>
      <h1>Happy Birthday!</h1>
      <p>사랑하는 경훈 오빠의 28번째 생일을 축하합니다.</p>
      <button class="gift-btn" @click="startGlitch">🎁 생일 선물 받기 🎁</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['finish'])
const isFading = ref(false)

const startGlitch = () => {
  isFading.value = true
  setTimeout(() => {
    emit('finish') // App.vue에게 끝났다고 알림
  }, 400)
}
</script>

<style scoped>
/* 💡 컴포넌트 자체를 뷰포트 전체 화면(풀 스크린)으로 만듭니다 */
.pink-full-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #fff5f5; /* 핑크색 배경 */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}
.fake-birthday-box {
  text-align: center;
  border: 2px solid #ffb3b3;
  background-color: #ffffff;
  color: #ff6666;
  padding: 40px;
  border-radius: 8px;
  box-shadow: 0 4px 25px rgba(255, 179, 179, 0.4);
}
.gift-btn {
  background-color: #ff6666;
  color: white;
  border: none;
  padding: 14px 28px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 30px;
  cursor: pointer;
  margin-top: 25px;
}
.fade-out {
  opacity: 0;
  transform: scale(0.95);
  transition: all 0.4s ease;
}
</style>