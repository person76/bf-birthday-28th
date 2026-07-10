<template>
  <div class="secure-box video-auth-container animate-fade-in">
    <span class="secure-badge blink">🚨 FATAL DATABASE LEAK</span>
    <h2>🔓 실시간 데이터베이스 유출 🔓</h2>

    <p class="desc">
      보안 서버 깊숙한 곳에서 남자친구의 과거 <br />
      디지털 발자국이 감지되었습니다.
    </p>

    <!-- 🎬 10초 막춤 동영상 플레이어 영역 -->
    <div class="video-wrapper">
      <video
          :src="danceVideo"
          autoplay
          loop
          muted
          playsinline
          class="hacker-video"
      ></video>
    </div>

    <p class="warning-text">⚠️ 위 영상 속 유경훈이 본인임을 인정하십니까?</p>

    <div class="btn-group">
      <button class="accept-btn" @click="handleAccept">
        🙋‍♂️ 인정합니다
      </button>
      <button class="deny-btn" @click="handleDeny">
        🙅‍♂️ 저 아닙니다 (누가 봐도...)
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import danceVideo from '@/assets/IMG_0847.MOV'

const emit = defineEmits(['finish', 'wrong'])

const handleAccept = () => {
  // 인정하면 다음 단계(전화 걸기)로 이동
  emit('finish')
}

const handleDeny = () => {
  // 부정하면 알람창 띄우며 처음으로 튕겨내기
  emit('wrong', '❌ 명백한 증거가 존재합니다. 거짓말 감지로 인해 처음으로 리부팅됩니다.')
}
</script>

<style scoped>
.video-auth-container {
  text-align: center;
  max-width: 450px;
  width: 90%;
  padding: 35px 20px;
}

.video-wrapper {
  width: 100%;
  max-width: 260px;        /* 가로 폭을 살짝 줄여서 폰에서 아담하게 만듦 */
  height: 320px;           /* 💡 세로 길이를 고정하여 위아래를 제한! */
  margin: 20px auto;
  border: 2px solid #00ff66;
  border-radius: 12px;
  overflow: hidden;        /* 💡 높이를 벗어나는 위아래 영상을 잘라버림(크롭) */
  box-shadow: 0 0 15px rgba(0, 255, 102, 0.2);
  background-color: #000;
}

.hacker-video {
  width: 100%;
  height: 100%;
  object-fit: cover;       /* 💡 중요: 영상이 찌그러지지 않고 꽉 차게 채우면서 정가운데만 보여줌 */
}

.warning-text {
  color: #ff3333;
  font-size: 13px;
  font-weight: bold;
  margin: 15px 0;
}

.btn-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-top: 20px;
}

.accept-btn {
  background-color: #00ff66;
  color: #000;
  border: none;
  padding: 14px;
  font-size: 14px;
  font-weight: bold;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.2s;
}
.accept-btn:hover { background-color: #00cc55; }

.deny-btn {
  background-color: #1a1a1a;
  color: #888;
  border: 1px solid #333;
  padding: 12px;
  font-size: 13px;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.2s;
}
.deny-btn:hover { border-color: #ff3333; color: #ff3333; }

.blink { animation: blinker 1.5s linear infinite; }
@keyframes blinker { 50% { opacity: 0; } }
.animate-fade-in { animation: fadeIn 0.4s ease forwards; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
</style>