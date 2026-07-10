<template>
  <div class="secure-box runaway-container animate-fade-in">
    <span class="secure-badge blink">CONSCIENCE TEST</span>
    <h2>🚨 양심 검증 🚨</h2>
    <p class="desc">
      당신은 당신의 여자친구를 얼마나 사랑합니까? <br />
      <span class="sub-desc">(정직하게 답변하지 않으면 시스템이 파괴됩니다.)</span>
    </p>

    <div class="btn-group">
      <!-- 💖 우주만큼 사랑함 버튼 (크기가 은근히 커짐) -->
      <button
          class="love-max-btn"
          :style="{ transform: `scale(${1 + clickCount * 0.08})` }"
          @click="handleSuccess"
      >
        ❤️ 우주가 무너져도 사랑함 ❤️
      </button>

      <!-- 💔 조금 사랑함 버튼 (누를 때마다 작아지고 글자가 바뀜) -->
      <button
          v-if="isFakeVisible"
          class="love-mini-btn"
          :style="{ scale: fakeScale, opacity: fakeOpacity }"
          @click="handleFakeClick"
      >
        {{ fakeText }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['finish'])

const clickCount = ref(0)
const isFakeVisible = ref(true)
const fakeScale = ref(1)
const fakeOpacity = ref(1)
const fakeText = ref('그냥 조금 사랑함')

// 킹받는 멘트 변화 리스트
const textStages = [
  '그냥 조금 사랑함',
  '앗 잘못 누름 진짜 조금임',
  '어라 버튼이 왜 작아지지?',
  '진심이야...?',
  '이래도 누른다고?',
  '누를 수 있으면 눌러봐ㅋ'
]

const handleFakeClick = () => {
  clickCount.value++

  // 1. 텍스트 변경
  if (clickCount.value < textStages.length) {
    fakeText.value = textStages[clickCount.value]
  }

  // 2. 크기와 투명도 서서히 축소
  fakeScale.value = Math.max(0, 1 - clickCount.value * 0.18)
  fakeOpacity.value = Math.max(0, 1 - clickCount.value * 0.15)

  // 3. 완전히 작아지면 버튼 삭제 (더 이상 못 누르게 격리)
  if (fakeScale.value <= 0.1) {
    isFakeVisible.value = false
  }
}

const handleSuccess = () => {
  // 우주만큼 사랑함을 누르면 최종 OTP 입력 단계로 이동!
  emit('finish')
}
</script>

<style scoped>
.runaway-container {
  text-align: center;
  max-width: 450px;
  width: 90%;
  padding: 40px 20px;
}

.sub-desc {
  font-size: 12px;
  color: #ff3333;
  display: block;
  margin-top: 5px;
}

.btn-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin-top: 35px;
  min-height: 160px; /* 버튼이 사라져도 레이아웃이 무너지지 않게 지탱 */
}

/* 정답 버튼: 누를수록 웅장해짐 */
.love-max-btn {
  background-color: #00ff66;
  color: #000;
  border: none;
  padding: 16px 24px;
  font-size: 15px;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.2s ease-out, background-color 0.2s;
  width: 100%;
  max-width: 320px;
  box-shadow: 0 0 15px rgba(0, 255, 102, 0.3);
}
.love-max-btn:hover {
  background-color: #00cc55;
}

/* 오답 버튼: 누를수록 작아지고 소멸함 */
.love-mini-btn {
  background-color: #1a1a1a;
  color: #ff3333;
  border: 1px solid #ff3333;
  padding: 12px 20px;
  font-size: 13px;
  font-weight: bold;
  border-radius: 6px;
  cursor: pointer;
  width: 100%;
  max-width: 280px;
  transition: all 0.15s ease-out;
  transform-origin: center; /* 가운데를 기준으로 줄어들게 설정 */
}

.blink {
  animation: blinker 1.5s linear infinite;
}
@keyframes blinker {
  50% { opacity: 0; }
}
</style>