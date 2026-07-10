<template>
  <div class="secure-box animate-fade-in">
    <span class="secure-badge">LEVEL 2 SECURITY</span>
    <h2>🔒 2차 OTP 인증 단계</h2>
    <p class="desc">
      보안 등급이 높아 추가 인증이 필요합니다.<br />
      지금 즉시 관리자(여친)에게 카톡으로 <br />
      <strong class="highlight">"인증번호 보내줘"</strong>라고 메시지를 보내 OTP 번호를 획득하십시오.
    </p>

    <div class="otp-input-container">
      <input
          v-model="otpInput"
          type="text"
          maxlength="4"
          placeholder="0000"
          @input="checkOTP"
      />
    </div>

    <p v-if="isWrong" class="error-msg">⚠️ 올바르지 않은 인증번호입니다. (해킹 시도 감지)</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 부모 컴포넌트인 SecurityContainer에게 성공했음을 알릴 이벤트 정의
const emit = defineEmits(['success'])

const otpInput = ref('')
const isWrong = ref(false)

// 💡 남자친구에게 카톡으로 알려줄 정답 OTP 번호를 지정하세요! (예: 생일 뒤 4자리)
const CORRECT_OTP = '0709'

const checkOTP = () => {
  // 4자리가 모두 입력되면 자동으로 체크 시작
  if (otpInput.value.length === 4) {
    if (otpInput.value === CORRECT_OTP) {
      isWrong.value = false
      emit('success') // 부모에게 성공 알림 -> 다음(성공) 단계로 이동
    } else {
      isWrong.value = true
      otpInput.value = '' // 틀리면 입력창 비우기
    }
  }
}
</script>

<style scoped>
/* SecurityContainer 내부의 .secure-box 스타일과 통일 */
.secure-box {
  text-align: center;
  border: 2px solid #00ff66;
  padding: 40px;
  border-radius: 8px;
  background-color: #121212;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.2);
  max-width: 450px;
  width: 100%;
}

.secure-badge {
  font-size: 12px;
  background: #ff3333;
  color: white;
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: bold;
}

.desc {
  color: #aaa;
  line-height: 1.6;
  margin: 20px 0;
  font-size: 14px;
}

.highlight {
  color: #fff;
  text-decoration: underline;
}

/* 큼직한 OTP 입력창 스타일 */
.otp-input-container {
  margin-top: 25px;
}

.otp-input-container input {
  background: #222;
  border: 1px solid #00ff66;
  color: #00ff66;
  font-size: 32px;
  text-align: center;
  padding: 10px;
  width: 160px;
  letter-spacing: 8px;
  border-radius: 4px;
  outline: none;
  font-family: 'Courier New', Courier, monospace;
}

.otp-input-container input::placeholder {
  color: #333;
}

.error-msg {
  color: #ff3333;
  font-size: 13px;
  margin-top: 15px;
}

.animate-fade-in {
  animation: fadeIn 0.4s ease forwards;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>