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
        @wrong="resetAll"
    />

    <RunawayView v-else-if="subStep === 'runaway'" @finish="subStep = 'video'"/>

    <VideoAuthView
        v-else-if="subStep === 'video'"
        @finish="subStep = 'otp'"
        @wrong="handleVideoWrong"
    />

    <!-- STEP 4: OTP 번호 입력 화면 -->
    <OtpView v-else-if="subStep === 'otp'" @success="subStep = 'success'"/>

    <!-- STEP 5: 최종 성공 화면 -->
    <SuccessBirthdayView v-else-if="subStep === 'success'"/>

    <!-- 🚨 사진이 동적으로 바뀌는 커스텀 알람창 -->
    <div v-if="isAlertOpen" class="alert-overlay">
      <div class="custom-alert-box">
        <div class="alert-header">🚨 SECURITY WARNING</div>
        <div class="alert-content">
          <!-- 💡 사진이 있을 때만 화면에 표시됨 -->
          <img v-if="alertImage" :src="alertImage" class="alert-img" alt="경고 이미지" />
          <p class="alert-body">{{ alertMessage }}</p>
        </div>
        <button class="alert-close-btn" @click="closeAlert">시스템 재부팅 (처음으로)</button>
      </div>
    </div>

  </div>
</template>

<script setup>
import {ref} from 'vue'
import Quiz from './Quiz.vue'
import RunawayView from './RunawayView.vue'
import OtpView from './OtpView.vue'
import SuccessBirthdayView from './SuccessBirthdayView.vue'
import VideoAuthView from "@/components/VideoAuthView.vue";

// 📸 퀴즈 보기용 사진들
import gfPic1 from '@/assets/돼지.jpeg'
import gfPic2 from '@/assets/강아지.png'
import gfPic3 from '@/assets/내사진.jpeg'
import gfPic4 from '@/assets/장원영.jpg'

// 🚨 [NEW] 오답 팝업창에 띄울 짤방/사진들 import (본인 파일명으로 고쳐주세요!)
import echoImg from '@/assets/ekko_fail.png'       // 에코 잘린 사진 혹은 트롤 사진
import stockImg from '@/assets/stock_fail.png'     // 주식 떡락 그래프 사진
import angryImg from '@/assets/angry_seoyeon.png'  // 서연님 삐진/화난 사진
import danceFailImg from '@/assets/angry_seoyeon.png' // 춤 거부했을 때 킹받는 사진
import robotImg from '@/assets/robot.png'
import angryHomeImg from '@/assets/angryHome.png'
import lol from '@/assets/lol.png'

const subStep = ref('intro')
const currentQuizIndex = ref(0)
const isAlertOpen = ref(false)
const alertMessage = ref('')
const alertImage = ref('') // 💡 알람창에 띄울 이미지 경로를 저장할 변수

const quizList = [
  {
    question: 'Q1. 다음 중 사라지면 가장 슬퍼할 것은?',
    options: ['롤 챔피언 에코', '반도체 주식', '임서연', '딱히 다 안 슬픔'],
    answerIndex: 2,
    hint: '힌트: 목숨은 하나뿐입니다. 신중하게 선택하세요.',
    wrongMessages: [
      '❌ 경훈이는 컴퓨터에서 에코랑 살아. 서연이는 집에 갈거야.',
      '❌ 임서연 애정 주식 상폐중... ',
      '',
      '❌ AI 감지 완료. 로그아웃합니다. 물론 내 마음에서'
    ],
    // 💡 [NEW] 각 보기(인덱스)에 맞는 오답 사진들을 순서대로 매핑!
    wrongImages: [
      echoImg,  // 0번 에코 선택 시
      stockImg, // 1번 주식 선택 시
      null,     // 2번은 정답이라 비워둠
      robotImg  // 3번 선택 시
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
      '❌ "지금 나한테 화낸거야?"\n 임서연은 화나서 집에 가버렸네요.',
      '❌ "어쩌라고."  \n임서연은 화나서 집에 가버렸네요.',
      '❌ "그러네, 우리 사이보다 확실히 좋아보이네" \n임서연은 화나서 집에 가버렸네요.',
      ''
    ],
    wrongImages: [
      angryHomeImg, // 0번 화낸다 선택 시
      angryHomeImg, // 1번 팩폭 선택 시
      angryHomeImg, // 2번 네트 선택 시
      null      // 3번 정답
    ]
  },
  {
    question: 'Q3. [보안 검증] 시스템에 등록된 데이터와 일치하는 당신의 진짜 여자친구를 고르십시오.',
    isImageQuiz: true,
    options: [gfPic1, gfPic2, gfPic3, gfPic4],
    answerIndex: 2,
    hint: '힌트: 세상에서 가장 예쁜 사람을 고르면 보안이 해제됩니다.',
    wrongMessages: [
      '❌ 죽을래? ',
      '❌ 귀여움 정도는 비슷하지만 아쉽게 아니네요.',
      '',
      '❌ 놀랄정도로 닮았지만 아쉽게 아니네요.'
    ],
    wrongImages: [
      angryImg, // 돼지 선택 시
      lol, // 강아지 선택 시
      null,     // 정답
      lol  /* 장원영 선택 시 (다른 예쁜 짤방이 있다면 그걸 넣어도 좋아요!) */
    ]
  }
]

const handleCorrect = () => {
  if (currentQuizIndex.value < quizList.length - 1) {
    currentQuizIndex.value++
  } else {
    subStep.value = 'runaway'
  }
}

// 💡 퀴즈 오답 처리 (글자와 매칭된 사진을 동시에 팝업에 띄움)
const resetAll = (optionIndex) => {
  const targetQuiz = quizList[currentQuizIndex.value]

  if (optionIndex === undefined || optionIndex === null || typeof optionIndex === 'object') {
    alertMessage.value = '❌ 오답입니다! 본인 인증에 실패하여 처음으로 돌아갑니다.'
    alertImage.value = angryImg // 기본 에러 사진
  } else {
    alertMessage.value = targetQuiz.wrongMessages[optionIndex] || '❌ 본인 인증에 실패했습니다.'
    // 💡 선택한 보기에 등록된 오답 사진을 팝업창 이미지로 세팅!
    alertImage.value = targetQuiz.wrongImages[optionIndex] || null
  }
  isAlertOpen.value = true
}

// 🎬 비디오 인증에서 "저 아닙니다" 발뺌했을 때 처리
const handleVideoWrong = (msg) => {
  alertMessage.value = msg
  alertImage.value = danceFailImg // 💡 춤 거부 전용 킹받는 사진 세팅!
  isAlertOpen.value = true
}

const closeAlert = () => {
  isAlertOpen.value = false
  subStep.value = 'intro'
  currentQuizIndex.value = 0
  alertImage.value = '' // 알람창 닫을 때 사진 초기화
}
</script>

<style scoped>
/* 기본 블랙 풀 스크린 레이아웃 */
.black-full-screen {
  position: fixed;
  top: 0; left: 0; width: 100vw; height: 100vh;
  background-color: #0b0b0b; color: #00ff66;
  display: flex; justify-content: center; align-items: center; z-index: 1;
}

.secure-box {
  text-align: center; border: 2px solid #00ff66; padding: 40px;
  border-radius: 12px; background-color: #121212;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.15); max-width: 450px; width: 90%;
}

.secure-badge { font-size: 12px; background: #ff3333; color: white; padding: 4px 8px; border-radius: 4px; font-weight: bold; }
.desc { color: #aaa; line-height: 1.6; margin: 20px 0; font-size: 14px; }
.next-btn { background-color: #00ff66; color: black; border: none; padding: 12px 24px; font-size: 16px; font-weight: bold; border-radius: 6px; cursor: pointer; margin-top: 20px; transition: 0.2s; }
.next-btn:hover { background-color: #00cc55; }

/* 🚨 알람창 오버레이 및 박스 */
.alert-overlay {
  position: fixed;
  top: 0; left: 0; width: 100vw; height: 100vh;
  background: rgba(0, 0, 0, 0.85);
  display: flex; justify-content: center; align-items: center;
  z-index: 9999999 !important;
}

.custom-alert-box {
  background: #151515; border: 2px solid #ff3333;
  box-shadow: 0 0 30px rgba(255, 51, 51, 0.3); border-radius: 8px;
  width: 90%; max-width: 400px; text-align: center; overflow: hidden;
  animation: shake 0.4s ease-in-out;
}

.alert-header { background: #ff3333; color: #fff; padding: 10px; font-weight: bold; font-size: 14px; letter-spacing: 1px; }

/* 💡 [NEW] 팝업 내부 정렬용 스타일 */
.alert-content {
  padding: 25px 20px 15px 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
}

/* 💡 [NEW] 알람창 내부 사진 예쁘게 다듬기 */
.alert-img {
  width: 100%;
  max-width: 220px; /* 폰에서 너무 커지지 않게 제한 */
  height: auto;
  border-radius: 8px;
  border: 1px solid #ff3333;
  object-fit: cover;
}

.alert-body { color: #fff; line-height: 1.6; font-size: 15px; margin: 0; white-space: pre-wrap; }
.alert-close-btn { background: #222; border: none; border-top: 1px solid #333; color: #ff3333; width: 100%; padding: 15px; font-size: 14px; font-weight: bold; cursor: pointer; }
.alert-close-btn:hover { background: #ff3333; color: #fff; }

@keyframes shake { 0%, 100% { transform: translateX(0); } 20%, 60% { transform: translateX(-6px); } 40%, 80% { transform: translateX(6px); } }
.animate-fade-in { animation: fadeIn 0.4s ease forwards; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
</style>