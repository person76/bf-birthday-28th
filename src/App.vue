<template>
  <FakeBirthdayView v-if="step === 'birthday'" @finish="step = 'security'" />

  <SecurityContainer v-else-if="step === 'security'" />
</template>

<script setup>
import { ref } from 'vue'
import FakeBirthdayView from './components/FakeBirthdayView.vue'
import SecurityContainer from './components/SecurityContainer.vue'

const step = ref('birthday') // birthday -> security
</script>

<style>
/* 1. 구글 폰트에서 배달의민족 도현체 가져오기 */
@import url('https://fonts.googleapis.com/css2?family=Do+Hyeon&display=swap');

/* 2. 모든 글자, 버튼, 입력창에 전역 적용 */
*, *::before, *::after, html, body, button, input, select, textarea {
  font-family: 'Do Hyeon', sans-serif !important;
  /* 도현체는 기본적으로 아주 두껍지만, 브라우저에 따라 한 번 더 두껍게 강조 처리 */
  font-weight: 400 !important;
  letter-spacing: 0.5px; /* 글자 간격 살짝 조정 */
}
</style>