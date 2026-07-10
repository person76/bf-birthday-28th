<template>
  <div class="secure-box otp-container animate-fade-in">
    <span class="secure-badge">FINAL STEP</span>
    <h2>📞 최종 2차 음성 인증 📞</h2>

    <div class="hacker-timer">
      <p class="timer-title">인증 유효 시간</p>
      <p class="timer-clock">{{ formatTime(timeLeft) }}</p>
    </div>

    <p class="desc">
      안전한 본인 확인을 위해 시스템 관리자(임서연)의 <br />
      <strong>음성 암호 승인</strong>이 필요합니다. <br />
      아래 버튼을 눌러 관리자에게 직접 암호를 요청하세요.
    </p>

    <!-- 🚨 서연님의 실제 전화번호로 변경해 주세요! -->
    <a href="tel:010-7157-3451" class="phone-call-btn" @click="handleCallClick">
      🤙 관리자에게 음성 암호 요청하기
    </a>

    <!-- 🔒 전화를 걸고 나면 비밀번호 입력창과 확인 버튼이 스르륵 나타납니다 -->
    <div v-if="hasCalled" class="input-zone animate-fade-in">
      <p class="input-guide">🔊 승인받은 암호 4자리를 입력하십시오.</p>

      <input
          v-model="inputPassword"
          type="password"
          maxlength="4"
          placeholder="••••"
          class="hacker-input"
          @keyup.enter="checkPassword"
      />

      <button class="success-bridge-btn" @click="checkPassword">
        🔓 시스템 잠금 해제 승인
      </button>

      <!-- 틀렸을 때만 뜨는 경고 문구 -->
      <p v-if="isWrong" class="error-msg animate-shake">
        ❌ 암호가 일치하지 않습니다. 다시 입력하세요.
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const emit = defineEmits(['success'])

const timeLeft = ref(180)
const hasCalled = ref(false)
const inputPassword = ref('')
const isWrong = ref(false)

// 💡 2차 인증 비밀번호 설정
const CORRECT_PASSWORD = '0313'

const formatTime = (seconds) => {
  const mins = Math.floor(seconds / 60)
  const secs = seconds % 60
  return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`
}

const handleCallClick = () => {
  hasCalled.value = true
  isWrong.value = false
}

// 💡 입력한 비밀번호 검증 함수
const checkPassword = () => {
  if (inputPassword.value === CORRECT_PASSWORD) {
    isWrong.value = false
    emit('success') // 정답이면 핑크 화면으로 점프!
  } else {
    isWrong.value = true // 틀리면 에러 메시지 띄우기
    inputPassword.value = '' // 입력창 초기화
  }
}

let timerInterval = null
onMounted(() => {
  timerInterval = setInterval(() => {
    if (timeLeft.value > 0) {
      timeLeft.value--
    } else {
      timeLeft.value = 180
    }
  }, 1000)
})

onUnmounted(() => {
  clearInterval(timerInterval)
})
</script>

<style scoped>
.otp-container {
  text-align: center;
  max-width: 450px;
  width: 90%;
  padding: 40px 20px;
}

.hacker-timer {
  background-color: #000;
  border: 1px solid #ff3333;
  padding: 15px;
  border-radius: 8px;
  margin: 20px auto;
  width: 60%;
  box-shadow: 0 0 10px rgba(255, 51, 51, 0.2);
}
.timer-title { font-size: 11px; color: #888; margin: 0; text-transform: uppercase; letter-spacing: 1px; }
.timer-clock { font-size: 28px; color: #ff3333; font-weight: bold; margin: 5px 0 0 0; font-family: 'Courier New', Courier, monospace; }

.desc { font-size: 14px; line-height: 1.6; color: #aaa; margin-bottom: 30px; }

.phone-call-btn {
  display: block;
  background-color: #ff3333;
  color: #fff;
  text-decoration: none;
  padding: 16px 20px;
  font-size: 15px;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 0 15px rgba(255, 51, 51, 0.3);
  transition: all 0.2s;
  margin-bottom: 25px;
}
.phone-call-btn:hover { background-color: #cc2222; }

/* 🔒 [NEW] 입력 영역 스타일 */
.input-zone {
  border-top: 1px dashed #333;
  padding-top: 25px;
  margin-top: 20px;
}
.input-guide {
  font-size: 13px;
  color: #00ff66;
  margin-bottom: 12px;
  font-weight: bold;
}
.hacker-input {
  background: #000;
  border: 2px solid #333;
  color: #00ff66;
  font-size: 24px;
  text-align: center;
  padding: 10px;
  width: 50%;
  border-radius: 6px;
  letter-spacing: 8px;
  margin-bottom: 15px;
  font-family: 'Courier New', Courier, monospace;
}
.hacker-input:focus {
  outline: none;
  border-color: #00ff66;
  box-shadow: 0 0 10px rgba(0, 255, 102, 0.3);
}

.success-bridge-btn {
  background-color: #00ff66;
  color: #000;
  border: none;
  padding: 16px 20px;
  font-size: 15px;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  width: 100%;
  box-shadow: 0 0 15px rgba(0, 255, 102, 0.3);
}
.success-bridge-btn:hover { background-color: #00cc55; }

.error-msg {
  color: #ff3333;
  font-size: 12px;
  margin-top: 12px;
  font-weight: bold;
}

/* 애니메이션 효과 */
.animate-fade-in { animation: fadeIn 0.4s ease forwards; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

.animate-shake { animation: shake 0.3s ease-in-out; }
@keyframes shake {
  0%, 100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-4px);
  }
  75% {
    transform: translateX(4px);
  }
}
</style>