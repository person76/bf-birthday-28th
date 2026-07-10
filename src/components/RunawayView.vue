<template>
  <div ref="containerRef" class="secure-box animate-fade-in">
    <h2>❤️ 마지막 양심 테스트</h2>
    <p class="question-text">현재 당신은 여친을 얼마나 사랑하고 있습니까?</p>

    <div class="btn-group">
      <button class="love-btn" @click="$emit('finish')">우주만큼 사랑함 (인증 완료)</button>

      <button
          class="hate-btn"
          :style="btnStyle"
          @mouseover="runAway"
          @click="runAway"
      >
        조금 사랑함
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

defineEmits(['finish'])

const containerRef = ref(null)

// 💡 처음에는 제자리에 평범하게 있다가 움직이도록 설정
const btnStyle = ref({
  position: 'relative',
  top: '0px',
  left: '0px'
})

const runAway = () => {
  if (!containerRef.value) return

  // 1. 초록색 상자(부모 박스)의 실제 크기와 패딩 공간을 계산합니다.
  const containerWidth = containerRef.value.clientWidth
  const containerHeight = containerRef.value.clientHeight

  // 2. 버튼이 박스 테두리를 뚫고 나가지 않도록 안전 범위를 좁게 잡습니다.
  // 상자 정중앙 기준 가로 ±120px, 세로 -30px ~ +80px 범위 내에서만 움직이게 제약합니다.
  const minX = -100
  const maxX = 100
  const minY = -20
  const maxY = 60

  // 3. 지정된 좁은 범위 안에서 무작위 좌표를 생성합니다.
  const randomLeft = Math.floor(Math.random() * (maxX - minX + 1)) + minX
  const randomTop = Math.floor(Math.random() * (maxY - minY + 1)) + minY

  // 4. position을 relative로 유지하여 상자 내부 안에서만 좌표가 변하도록 합니다.
  btnStyle.value.position = 'relative'
  btnStyle.value.left = `${randomLeft}px`
  btnStyle.value.top = `${randomTop}px`
}
</script>

<style scoped>
.secure-box {
  position: relative; /* 자식 요소들의 기준점 역할 */
  text-align: center;
  border: 2px solid #00ff66;
  padding: 40px;
  border-radius: 8px;
  background-color: #121212;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.2);
  max-width: 450px;
  width: 100%;
  min-height: 280px; /* 버튼이 상하로 움직일 공간 확보 */
  overflow: hidden;  /* 혹시나 경계를 넘어가면 깔끔하게 가려줌 */
}

.question-text {
  font-size: 18px;
  margin: 20px 0 30px 0;
  color: #fff;
  line-height: 1.5;
}

.btn-group {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  margin-top: 40px;
}

button {
  padding: 12px 20px;
  font-size: 15px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-weight: bold;
  font-family: 'Courier New', Courier, monospace;
}

.love-btn {
  background: #00ff66;
  color: #000;
  z-index: 10; /* 정답 버튼이 무조건 위에 오도록 보장 */
}

/* 💡 도망가는 버튼 스타일 핵심 수정 */
.hate-btn {
  background: #333;
  color: #888;
  z-index: 5;
  /* 0.25초 동안 스르륵 미끄러지듯 도망가서 움직임이 눈에 선명하게 보입니다 */
  transition: transform 0.2s ease, left 0.25s cubic-bezier(0.25, 0.8, 0.25, 1), top 0.25s cubic-bezier(0.25, 0.8, 0.25, 1);
}

.animate-fade-in { animation: fadeIn 0.4s ease forwards; }
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>