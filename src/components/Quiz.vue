<template>
  <div class="quiz-box">
    <span class="secure-badge">LEVEL 1 SECURITY</span>
    <span class="progress">{{ currentIndex + 1 }} / {{ totalQuizzes }}</span>

    <h3 class="question">{{ quizData.question }}</h3>

    <div class="options-container">
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
    // 💡 중요: 틀렸을 때 클릭한 보기의 번호(index)를 부모에게 같이 던져줍니다.
    emit('wrong', index)
  }
}
</script>

<style scoped>
/* 부모 컴포넌트(.box)와 스타일 감성을 맞춤 */
.quiz-box {
  position: relative;
  text-align: center;
  border: 2px solid #00ff66;
  padding: 40px;
  border-radius: 8px;
  background-color: #121212;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.2);
  max-width: 450px;
  width: 100%;
}
.progress {
  position: absolute;
  top: 15px;
  right: 20px;
  font-size: 14px;
  color: #888;
}
.question {
  margin: 30px 0;
  font-size: 18px;
  line-height: 1.5;
  color: #fff;
}
.options-container {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.option-btn {
  background-color: #1a1a1a;
  color: #00ff66;
  border: 1px solid #333;
  padding: 14px;
  font-size: 15px;
  border-radius: 6px;
  cursor: pointer;
  text-align: left;
  transition: all 0.2s;
  font-family: 'Courier New', Courier, monospace;
}
.option-btn:hover {
  background-color: #222;
  border-color: #00ff66;
  transform: translateX(5px);
  box-shadow: 0 0 10px rgba(0, 255, 102, 0.1);
}
.hint-text {
  margin-top: 25px;
  font-size: 13px;
  color: #888;
  cursor: pointer;
  text-decoration: underline;
}
.hint-text:hover {
  color: #00ff66;
}
</style>