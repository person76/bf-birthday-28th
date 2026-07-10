<template>
  <div class="quiz-box">
    <span class="secure-badge">LEVEL 1 SECURITY</span>
    <span class="progress">{{ currentIndex + 1 }} / {{ totalQuizzes }}</span>

    <h3 class="question">{{ quizData.question }}</h3>

    <div v-if="quizData.isImageQuiz" class="image-options-grid">
      <div
          v-for="(option, index) in quizData.options"
          :key="index"
          class="image-option-card"
          @click="selectOption(index)"
      >
        <img :src="option" alt="선택지 사진" class="quiz-img" />
        <div class="image-number">{{ index + 1 }}</div>
      </div>
    </div>

    <div v-else class="options-container">
      <button
          v-for="(option, index) in quizData.options"
          :key="index"
          class="option-btn"
          @click="selectOption(index)"
      >
        {{ option }}
      </button>
    </div>

    <p class="hint-text" @click="showHint = !showHint">
      {{ showHint ? quizData.hint : '💡 기밀 힌트 보기' }}
    </p>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  quizData: Object,
  currentIndex: Number,
  totalQuizzes: Number
})

const emit = defineEmits(['correct', 'wrong'])
const showHint = ref(false)

watch(() => props.currentIndex, () => {
  showHint.value = false
})

const selectOption = (index) => {
  if (index === props.quizData.answerIndex) {
    emit('correct')
  } else {
    emit('wrong', index)
  }
}
</script>

<style scoped>
.quiz-box {
  position: relative;
  text-align: center;
  border: 2px solid #00ff66;
  padding: 40px;
  border-radius: 8px;
  background-color: #121212;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.2);
  max-width: 480px; /* 사진 4개를 위해 가로폭을 살짝 늘렸습니다 */
  width: 100%;
}
.progress { position: absolute; top: 15px; right: 20px; font-size: 14px; color: #888; }
.question { margin: 30px 0; font-size: 18px; line-height: 1.5; color: #fff; }

/* 기존 글자 버튼 스타일 */
.options-container { display: flex; flex-direction: column; gap: 12px; }
.option-btn {
  background-color: #1a1a1a; color: #00ff66; border: 1px solid #333;
  padding: 14px; font-size: 15px; border-radius: 6px; cursor: pointer;
  text-align: left; transition: all 0.2s;
}
.option-btn:hover { background-color: #222; border-color: #00ff66; transform: translateX(5px); }

/* 📸 [NEW] 사진 보기용 2x2 그리드 스타일 */
.image-options-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  margin-bottom: 20px;
}
.image-option-card {
  position: relative;
  border: 1px solid #333;
  border-radius: 8px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.2s;
  aspect-ratio: 1 / 1; /* 정사각형 유지 */
  background: #000;
}
.image-option-card:hover {
  border-color: #00ff66;
  box-shadow: 0 0 15px rgba(0, 255, 102, 0.3);
  transform: scale(1.03);
}
.quiz-img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 사진이 찌그러지지 않게 채움 */
}
.image-number {
  position: absolute;
  bottom: 8px; right: 8px;
  background: rgba(0, 0, 0, 0.7);
  color: #00ff66;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: bold;
}

.hint-text { margin-top: 25px; font-size: 13px; color: #888; cursor: pointer; text-decoration: underline; }
.hint-text:hover { color: #00ff66; }
</style>