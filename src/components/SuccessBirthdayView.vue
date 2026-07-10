<template>
  <div class="pink-full-screen animate-fade-in">
    <div class="birthday-card animate-scale-up">
      <span class="birthday-badge">🎂 THE HERO OF THE DAY</span>
      <h2>🎉 HAPPY BIRTHDAY 🎉</h2>
      <p class="greet">유경훈 인증 완료!</p>
      <p class="greet-sub">오늘 하루 세상에서 가장 행복한 주인공이 되길 바라</p>

      <!-- 📸 [NEW] 서연님이 주신 쿠폰 이미지 들어가는 영역 -->
      <div class="coupon-container">
        <img :src="couponImg" alt="여자친구비 면제 쿠폰" class="coupon-img" />
      </div>
    </div>
  </div>
</template>

<script setup>
// assets 폴더에 넣은 쿠폰 이미지를 불러옵니다.
import couponImg from '@/assets/여자친구비 면제쿠폰.jpg'
</script>

<style scoped>
/* 🌸 화면 전체를 꽉 채우는 핑크 배경 레이아웃 */
.pink-full-screen {
  position: fixed;
  top: 0; left: 0; width: 100vw; height: 100vh;
  background: linear-gradient(135deg, #ffeef2 0%, #ffdeeb 100%);
  display: flex; justify-content: center; align-items: center;
  z-index: 999999;
  overflow-y: auto; /* 화면이 작은 폰을 위해 스크롤 허용 */
  padding: 20px 0;
}

/* 🤍 중앙에 뜨는 고급스럽고 두툼한 생일 카드 디자인 */
.birthday-card {
  text-align: center;
  background-color: #ffffff;
  border: 3px solid #ff6699;
  padding: 40px 30px;
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(255, 102, 153, 0.2);
  max-width: 440px;
  width: 90%;
  max-height: 90vh; /* 화면 밖으로 튀어나가지 않게 조절 */
  overflow-y: auto;
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
  font-size: 28px;
  color: #ff3366;
  margin: 15px 0 5px 0;
}

.greet { font-size: 18px; font-weight: bold; color: #222; margin-bottom: 5px; }
.greet-sub { font-size: 13px; color: #666; margin-bottom: 20px; line-height: 1.4; }

/* 📸 쿠폰 이미지 스타일링 */
.coupon-container {
  width: 70%;              /* 💡 가로폭을 70%로 축약하여 아담하게 설정 */
  margin: 0 auto 20px auto; /* 💡 가운데 정렬 및 아래 여백 조정 */
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  border: 1px solid #ffd2e1;
}
.coupon-img {
  width: 100%;
  height: auto;
  display: block;
}

/* 🎁 보상 내역 박스 */
.gift-box {
  background: #fff5f8;
  border: 2px dashed #ff99bb;
  padding: 20px 15px;
  border-radius: 12px;
  text-align: left;
}

.gift-box h3 { color: #ff3366; margin-bottom: 12px; font-size: 15px; text-align: center; }
.gift-item { font-size: 13px; margin: 8px 0; color: #444; font-weight: 600; }
.love-msg {
  color: #ff0055; font-size: 14px; font-weight: bold; margin-top: 15px;
  border-top: 1px solid #ffd2e1; padding-top: 12px; text-align: center;
}

/* ✨ 애니메이션 */
.animate-fade-in { animation: fadeIn 0.6s ease forwards; }
.animate-scale-up { animation: scaleUp 0.5s cubic-bezier(0.34, 1.56, 0.64, 1) forwards; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
@keyframes scaleUp { from { opacity: 0; transform: scale(0.9); } to { opacity: 1; transform: scale(1); } }

/* 스크롤바 숨기기 */
.birthday-card::-webkit-scrollbar { display: none; }
</style>