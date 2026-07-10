<template>
  <div class="black-full-screen">

    <div v-if="subStep === 'intro'" class="secure-box animate-fade-in">
      <span class="secure-badge">SYSTEM ERROR</span>
      <h2>🚨 어... 잠깐만... 🚨</h2>
      <p class="desc">
        최근 내 남자친구의 귀여움과 다정함이 한도를 초과하여,<br />
        <strong style="color: #fff;">외부 해킹(또는 로봇)</strong>으로 의심되는 정황이 포착되었습니다.
      </p>
      <p class="desc" style="color: #ff3333; font-weight: bold;">
        당신은 정말 내 남자친구가 맞습니까?
      </p>
      <button class="next-btn" @click="subStep = 'quiz'">본인 인증 시작하기</button>
    </div>

    <Quiz
        v-else-if="subStep === 'quiz'"
        :quiz-data="quizList[currentQuizIndex]"
        :current-index="currentQuizIndex"
        :total-quizzes="quizList.length"
        @correct="handleCorrect"
        @wrong="(idx) => resetAll(idx)"
    />

    <RunawayView v-else-if="subStep === 'runaway'" @finish="subStep = 'otp'" />

    <!-- STEP 4: OTP 번호 입력 화면 -->
    <OtpView v-else-if="subStep === 'otp'" @success="subStep = 'success'" />

    <!-- STEP 5: 최종 성공 화면 (💡 컴포넌트 호출로 깔끔하게 변경!) -->
    <SuccessBirthdayView v-else-if="subStep === 'success'" />

  </div>
</template>

<script setup>
import { ref } from 'vue'
import Quiz from './Quiz.vue'
import RunawayView from './RunawayView.vue'
import OtpView from './OtpView.vue'
import SuccessBirthdayView from './SuccessBirthdayView.vue' // 💡 여기에 추가!

// 💡 subStep 단계 축소: intro -> quiz -> runaway -> otp -> success
const subStep = ref('intro')
const currentQuizIndex = ref(0)

const isAlertOpen = ref(false)
const alertMessage = ref('')

const quizList = [
  {
    question: 'Q1. 다음 중 사라지면 가장 슬퍼할 것은?',
    options: ['롤 챔프 에코', '반도체 주식', '임서연', '딱히 다 안 슬픔'],
    answerIndex: 2,
    hint: '힌트: 목숨은 하나뿐입니다. 신중하게 선택하세요.',
    wrongMessages: [
      '❌ 경훈이는 컴퓨터에서 에코랑 살아. 서연이는 집에 갈거야.',
      '❌ 임서연 애정 주식 상폐중... ',
      '',
      '❌ AI 감지 완료. 로그아웃합니다. 물론 내 마음에서'
    ]
  },
  {
    question: 'Q2. 혼복 배드민턴 경기 중 나의 말도 안되는 실수로 다 이긴 경기를 져버렸다. 이때 당신의 반응은? ',
    options: [
      '"아니 뭐하는데!!!" 하며 화낸다.',
      '"서연아, 아까 전위에서 라켓 들고만 있어도 이겼어." 라며 팩트 폭행 한다.',
      '"이야, 어떻게 드롭 치는 족족 네트에만 걸리지? 우리 사이보다 네트랑 콕 사이가 더 좋네!" 라며 놀린다.',
      '"괜찮아, 다음에 또 나가면 되지. 맛있는거나 먹으러 가자."라며 삼쏘 하러 간다.'
    ],
    answerIndex: 3,
    hint: '힌트: 목숨은 하나뿐입니다. 신중하게 선택하세요.',
    wrongMessages: [
      '❌ "지금 나한테 화낸거야?" 임서연은 화나서 집에 가버렸네요.',
      '❌ "어쩌라고."  임서연은 화나서 집에 가버렸네요.',
      '❌ "그러네, 우리 사이보다 확실히 좋아보이네" 임서연은 화나서 집에 가버렸네요."',
      ''
    ]
  }
]

const handleCorrect = () => {
  if (currentQuizIndex.value < quizList.length - 1) {
    currentQuizIndex.value++
  } else {
    // 💡 수정됨: 모든 퀴즈를 맞추면 바로 runaway(도망가는 버튼)로 점프!
    subStep.value = 'runaway'
  }
}

const resetAll = (optionIndex) => {
  const targetQuiz = quizList[currentQuizIndex.value]
  alertMessage.value = targetQuiz.wrongMessages[optionIndex] || '❌ 본인 인증에 실패했습니다.'
  isAlertOpen.value = true
}

const closeModal = () => {
  isAlertOpen.value = false
}

const closeAlert = () => {
  isAlertOpen.value = false
  subStep.value = 'intro'
  currentQuizIndex.value = 0
}
</script>
<style scoped>
/* 기본 블랙 풀 스크린 레이아웃 */
.black-full-screen {
  position: fixed;
  top: 0; left: 0; width: 100vw; height: 100vh;
  background-color: #0b0b0b;
  color: #00ff66;
  display: flex; justify-content: center; align-items: center;
}

/* 기본 보안 상자 디자인 */
.secure-box {
  text-align: center;
  border: 2px solid #00ff66;
  padding: 40px;
  border-radius: 12px;
  background-color: #121212;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.15);
  max-width: 450px;
  width: 90%;
}

.secure-badge { font-size: 12px; background: #ff3333; color: white; padding: 4px 8px; border-radius: 4px; font-weight: bold; }
.desc { color: #aaa; line-height: 1.6; margin: 20px 0; font-size: 14px; }
.next-btn { background-color: #00ff66; color: black; border: none; padding: 12px 24px; font-size: 16px; font-weight: bold; border-radius: 6px; cursor: pointer; margin-top: 20px; transition: 0.2s; }
.next-btn:hover { background-color: #00cc55; }

/* ---------------------------------------------------------------- */
/* 🎀 [NEW] 최종 성공 시 반전되는 핑크색 페이지 스타일 */
/* ---------------------------------------------------------------- */
.secure-box.pink-version {
  background-color: #fff0f5;  /* 화사한 라벤더 핑크 배경 */
  border: 3px solid #ff6699;  /* 진한 핑크색 테두리 */
  color: #ff3366;             /* 텍스트 핑크색 메인 컬러 */
  box-shadow: 0 0 30px rgba(255, 102, 153, 0.3); /* 핑크빛 네온 효과 */
}

.pink-version h2 {
  font-size: 28px;
  color: #ff3366;
  margin-bottom: 10px;
  text-shadow: 1px 1px 0px rgba(0, 0, 0, 0.1);
}

.pink-version .greet {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.pink-version .greet-sub {
  font-size: 14px;
  color: #666;
  margin-bottom: 25px;
  line-height: 1.4;
}

/* 핑크 버전 내부 기프트 카드 */
.pink-version .gift-card {
  background: #ffffff;
  border: 2px dashed #ff6699;
  padding: 25px 20px;
  border-radius: 8px;
  color: #444;
}

.pink-version .gift-card h3 {
  color: #ff3366;
  margin-bottom: 15px;
  font-size: 18px;
}

.pink-version .gift-item {
  font-size: 14px;
  margin: 8px 0;
  color: #555;
}

.pink-version .love-msg {
  color: #ff0055;
  font-size: 15px;
  font-weight: bold;
  margin-top: 20px;
  border-top: 1px solid #ffe6ee;
  padding-top: 15px;
}

/* 공통 알람창 및 애니메이션 스타일 */
.alert-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0, 0, 0, 0.85); display: flex; justify-content: center; align-items: center; z-index: 99999; }
.custom-alert-box { background: #151515; border: 2px solid #ff3333; box-shadow: 0 0 30px rgba(255, 51, 51, 0.3); border-radius: 8px; width: 90%; max-width: 400px; text-align: center; overflow: hidden; animation: shake 0.4s ease-in-out; }
.alert-header { background: #ff3333; color: #fff; padding: 10px; font-weight: bold; font-size: 14px; letter-spacing: 1px; }
.alert-body { color: #fff; padding: 30px 20px; line-height: 1.6; font-size: 15px; margin: 0; }
.alert-close-btn { background: #222; border: none; border-top: 1px solid #333; color: #ff3333; width: 100%; padding: 15px; font-size: 14px; font-weight: bold; cursor: pointer; }
.alert-close-btn:hover { background: #ff3333; color: #fff; }
@keyframes shake { 0%, 100% { transform: translateX(0); } 20%, 60% { transform: translateX(-6px); } 40%, 80% { transform: translateX(6px); } }
.animate-fade-in { animation: fadeIn 0.4s ease forwards; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
</style>