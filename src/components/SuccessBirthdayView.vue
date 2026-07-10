<template>
  <div class="pink-full-screen animate-fade-in">
    <!-- 화면 전체가 환한 파스텔 핑크 배경이 됩니다 -->
    <div class="birthday-card animate-scale-up">
      <span class="birthday-badge">🎂 THE HERO OF THE DAY</span>
      <h2>🎉 HAPPY BIRTHDAY 🎉</h2>
      <p class="greet">본인 인증 최종 승인 완료!</p>
      <p class="greet-sub">오늘 하루 세상에서 가장 행복한 주인공이 되길 바래</p>

      <div class="gift-box">
        <h3>🎁 생일 축하 보상 목록 🎁</h3>
        <p class="gift-item">✔️ 대형 소원권 1매 </p>
        <p class="gift-item">✔️ 롤 연패 케어 이용권 1매</p>
        <p class="love-msg">생일 진심으로 축하해! 내 남친이 돼줘서 고마워 ❤️</p>
      </div>
    </div>
  </div>
</template>

<script setup>
// 이 화면은 최종 화면이므로 부모에게 이벤트를 보낼 필요 없이 정보만 띄워줍니다.
</script>

<style scoped>
/* 🌸 화면 전체를 꽉 채우는 핑크 배경 레이아웃 */
.pink-full-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: linear-gradient(135deg, #ffeef2 0%, #ffdeeb 100%); /* 화사한 파스텔 핑크 그라데이션 */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999999; /* 해커 화면의 모든 요소를 완벽하게 덮어버림 */
}

/* 🤍 중앙에 뜨는 고급스럽고 두툼한 생일 카드 디자인 */
.birthday-card {
  text-align: center;
  background-color: #ffffff;
  border: 3px solid #ff6699;
  padding: 45px 40px;
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(255, 102, 153, 0.2);
  max-width: 480px;
  width: 90%;
}

.birthday-badge {
  font-size: 11px;
  background: #ff5588;
  color: white;
  padding: 5px 10px;
  border-radius: 20px;
  font-weight: bold;
  letter-spacing: 1px;
}

h2 {
  font-size: 32px;
  color: #ff3366;
  margin: 20px 0 10px 0;
}

.greet {
  font-size: 20px;
  font-weight: bold;
  color: #222;
  margin-bottom: 5px;
}

.greet-sub {
  font-size: 14px;
  color: #666;
  margin-bottom: 30px;
  line-height: 1.5;
}

/* 🎁 보상 내역 박스 */
.gift-box {
  background: #fff5f8;
  border: 2px dashed #ff99bb;
  padding: 25px 20px;
  border-radius: 12px;
  text-align: left;
}

.gift-box h3 {
  color: #ff3366;
  margin-bottom: 15px;
  font-size: 16px;
  text-align: center;
}

.gift-item {
  font-size: 14px;
  margin: 10px 0;
  color: #444;
  font-weight: 600;
}

.love-msg {
  color: #ff0055;
  font-size: 15px;
  font-weight: bold;
  margin-top: 25px;
  border-top: 1px solid #ffd2e1;
  padding-top: 15px;
  text-align: center;
}

/* ✨ 애니메이션 속도 제어로 등장 효과 극대화 */
.animate-fade-in {
  animation: fadeIn 0.6s ease forwards;
}

.animate-scale-up {
  animation: scaleUp 0.5s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
  animation-delay: 0.2s;
  opacity: 0;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes scaleUp {
  from { opacity: 0; transform: scale(0.9); }
  to { opacity: 1; transform: scale(1); }
}
</style>