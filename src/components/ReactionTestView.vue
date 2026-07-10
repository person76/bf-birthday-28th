<template>
  <div class="secure-box reaction-container animate-fade-in">
    <span class="secure-badge">🎮 LOL PIXEL REFLEX TEST</span>
    <h2>⚡ 롤 피지컬 생체 반응속도 측정 ⚡</h2>

    <p class="desc">
      최근 탐지된 로봇은 롤 실력이 형편없었습니다.<br />
      진짜 내 남자친구라면 <strong style="color: #00ff66;">다이아급 피지컬</strong>을 증명해야 합니다.<br />
      화면이 <strong style="color: #00ff66;">🟢 초록색</strong>으로 변하는 순간 점멸(Flash)을 쓰듯 빛의 속도로 클릭하세요.
    </p>

    <div
        :class="['test-screen', status]"
        @mousedown="handleBoxClick"
    >
      <div class="screen-text">
        <span v-if="status === 'idle'">🔒 인증 시스템 대기 중 (시작 버튼을 누르세요)</span>
        <span v-else-if="status === 'waiting'">🔴 대기 중... 마음을 가다듬으세요.</span>
        <span v-else-if="status === 'ready'">⚡ 지금 점멸(Flash) 쓰세요!!! NOW!!!</span>
        <span v-else-if="status === 'result'">📊 판정 진행 중...</span>
      </div>
    </div>

    <div v-if="status === 'result'" class="result-area">
      <p class="result-time">당신의 반응속도: <span>{{ reactionTime }}ms</span></p>
      <p class="fail-reason">🏆 내 남친 통과 기준: 200ms 이하</p>
    </div>

    <button
        v-if="status === 'idle'"
        class="start-btn"
        @click="startTest"
    >
      인증 시작 (정밀 측정)
    </button>
  </div>
</template>

<script setup>
import { ref, onUnmounted } from 'vue'

const emit = defineEmits(['finish', 'wrong'])

const status = ref('idle') // idle(시작 전), waiting(빨강), ready(초록), result(결과)
const timer = ref(null)
const startTime = ref(0)
const reactionTime = ref(0)

// 버튼을 눌러야 비로소 waiting(빨간색 화면) 상태가 시작됨
const startTest = () => {
  status.value = 'waiting'
  const randomDelay = Math.random() * 2000 + 1500 // 1.5초 ~ 3.5초 사이 랜덤

  timer.value = setTimeout(() => {
    status.value = 'ready'
    startTime.value = Date.now()
  }, randomDelay)
}

const handleBoxClick = () => {
  // 시작 버튼 누르기도 전에 검은 화면을 누르면 아무 반응 없게 차단
  if (status.value === 'idle') return

  if (status.value === 'waiting' && timer.value) {
    // 억까 1: 초록 불 들어오기 전에 급해서 미리 누른 경우 (예측샷 실패)
    clearTimeout(timer.value)
    timer.value = null
    emit('wrong', '❌ 부정 출발 감지!\n초록불도 안 켜졌는데 점멸을 빼다니요? 뇌절 플레이로 인해 처음으로 리부팅됩니다.')
  } else if (status.value === 'ready') {
    const endTime = Date.now()
    reactionTime.value = endTime - startTime.value
    status.value = 'result'

    // 💡 억까 2: 200ms 이하 실패 유도
    setTimeout(() => {
      if (reactionTime.value <= 200) {
        emit('finish')
      } else {
        emit('wrong', `❌ 반응속도 ${reactionTime.value}ms 측정 완료.\n\n이 속도로는 페이커는커녕 브론즈의 논타겟 스킬도 못 피합니다.\n롤 잘하는 내 남친일 리가 없습니다. 리부팅합니다.`)
      }
    }, 1200)
  }
}

onUnmounted(() => {
  if (timer.value) clearTimeout(timer.value)
})
</script>

<style scoped>
.reaction-container {
  text-align: center;
  max-width: 450px;
  width: 90%;
}

.test-screen {
  width: 100%;
  height: 200px;
  margin: 20px 0;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: background-color 0.05s ease;
  border: 2px dashed #333;
}

/* 상태별 배경색 디자인 */
.test-screen.idle { background-color: #0b0b0b; color: #444; border-style: dashed; }
.test-screen.waiting { background-color: #aa2222; border-style: solid; border-color: #ff3333; }
.test-screen.ready { background-color: #00aa44; border-style: solid; border-color: #00ff66; box-shadow: 0 0 20px rgba(0, 255, 102, 0.4); }
.test-screen.result { background-color: #222222; border-style: solid; border-color: #00ff66; }

.screen-text {
  font-size: 15px;
  font-weight: bold;
  user-select: none;
}
/* idle 상태일 때는 글자 색상을 어둡게 줍니다 */
.test-screen.idle .screen-text { color: #666; }
.test-screen.waiting .screen-text, .test-screen.ready .screen-text { color: #fff; }

.result-area {
  margin-top: 15px;
  animation: fadeIn 0.3s ease;
}

.result-time { font-size: 18px; color: #fff; }
.result-time span { color: #ff3333; font-weight: bold; font-size: 24px; }
.fail-reason { color: #888; font-size: 12px; margin-top: 5px; }

.start-btn {
  background-color: #00ff66; color: #000; border: none;
  padding: 14px 28px; font-size: 15px; font-weight: bold;
  border-radius: 6px; cursor: pointer; transition: 0.2s;
  box-shadow: 0 0 10px rgba(0, 255, 102, 0.2);
}
.start-btn:hover { background-color: #00cc55; }
</style>