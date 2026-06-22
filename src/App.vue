<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

import celadon    from '../resources/고려청자.png'
import gat        from '../resources/갓.png'
import hunmin     from '../resources/훈민정음.jpg'
import moonjar    from '../resources/백자달항아리.jpeg'
import seokguram  from '../resources/석굴암.jpg'

const current = ref(0)

const items = [
  {
    id: 1,
    img: celadon,
    char: '靑',
    name: '고려청자',
    nameEn: 'Goryeo Celadon',
    period: '918 — 1392',
    category: '陶瓷 · 도자기',
    desc: '고려시대를 대표하는 도자 예술품. 비취색이라 불리는 독특한 청록빛 유약과 정교한 상감기법이 특징으로, 12세기 당시 세계 최고 수준의 도예 기술을 보여준다. 현재 다수가 국보로 지정되어 있다.',
  },
  {
    id: 2,
    img: gat,
    char: '笠',
    name: '갓',
    nameEn: 'Gat · Traditional Hat',
    period: '조선시대 · 1392 — 1897',
    category: '服飾 · 복식',
    desc: '조선시대 성인 남성이 착용하던 전통 모자. 말총(말 꼬리털)을 엮어 만든 정교한 공예품으로, 착용자의 신분과 격식을 드러내는 중요한 의례용품이었다.',
  },
  {
    id: 3,
    img: hunmin,
    char: '訓',
    name: '훈민정음',
    nameEn: 'Hunminjeongeum',
    period: '1443년 창제',
    category: '文字 · 문자',
    desc: '세종대왕이 1443년 창제한 한국 고유의 문자 체계. 유네스코 세계기록유산으로 등재된 해례본은 한글의 창제 원리와 철학을 담은 세계 유일의 기록으로 평가받는다.',
  },
  {
    id: 4,
    img: moonjar,
    char: '白',
    name: '백자달항아리',
    nameEn: 'White Moon Jar',
    period: '17 — 18세기',
    category: '陶磁器 · 도자기',
    desc: '조선 후기 제작된 순백의 백자 항아리. 완벽한 구형을 추구하면서도 인위적이지 않은 자연스러운 형태에서 조선 미학의 정수인 여백과 절제의 미를 느낄 수 있다.',
  },
  {
    id: 5,
    img: seokguram,
    char: '佛',
    name: '석굴암',
    nameEn: 'Seokguram Grotto',
    period: '751년 · 통일신라',
    category: '石造建築 · 석조건축',
    desc: '경주 토함산에 위치한 통일신라시대 석굴 사원. 본존불을 중심으로 한 정밀한 수학적 설계와 조각 기술이 집약된 걸작으로, 유네스코 세계문화유산으로 등재되어 있다.',
  },
  {
    id: 6,
    char: '衣',
    name: '한복',
    nameEn: 'Hanbok · Traditional Dress',
    period: '삼국시대 이후',
    category: '服飾 · 복식',
    desc: '한국의 전통 의상. 직선과 유려한 곡선의 조화가 특징으로, 저고리와 치마(여성) 또는 바지(남성)로 구성된다. 색채와 형태에서 한국 고유 미학의 정수를 보여준다.',
  },
]

const prev = () => { if (current.value > 0) current.value-- }
const next = () => { if (current.value < items.length - 1) current.value++ }

const onKey = (e: KeyboardEvent) => {
  if (e.key === 'ArrowLeft') prev()
  if (e.key === 'ArrowRight') next()
}

// Touch swipe
let touchStartX = 0
const onTouchStart = (e: TouchEvent) => { touchStartX = e.touches[0].clientX }
const onTouchEnd = (e: TouchEvent) => {
  const dx = e.changedTouches[0].clientX - touchStartX
  if (dx > 50) prev()
  else if (dx < -50) next()
}

onMounted(() => window.addEventListener('keydown', onKey))
onUnmounted(() => window.removeEventListener('keydown', onKey))

// Modal
const showModal = ref(true)
const formMode = ref<'login' | 'signup' | 'forgot'>('login')

const loginEmail    = ref('')
const loginPassword = ref('')
const signupName    = ref('')
const signupEmail   = ref('')
const signupPassword  = ref('')
const signupConfirm   = ref('')
const forgotEmail   = ref('')

const loginError = ref('')

const closeModal = () => { showModal.value = false }
const switchTo = (mode: 'login' | 'signup' | 'forgot') => {
  formMode.value = mode
  loginError.value = ''
}

const handleLogin = () => {
  if (loginEmail.value === 'dmswls1089@idif.co.kr' && loginPassword.value === '1') {
    closeModal()
  } else {
    loginError.value = '이메일 또는 비밀번호가 올바르지 않습니다.'
  }
}
</script>

<template>
  <div class="app" @touchstart="onTouchStart" @touchend="onTouchEnd">

    <!-- HEADER -->
    <header class="header">
      <div class="header-left">
        <p class="header-en">Korean Cultural Heritage</p>
        <h1 class="header-ko">韓國의 시간을 걷다</h1>
      </div>
      <div class="header-right">
        <span class="counter">
          <em>{{ String(current + 1).padStart(2, '0') }}</em>
          <span class="counter-sep"> / {{ String(items.length).padStart(2, '0') }}</span>
        </span>
      </div>
    </header>

    <!-- GALLERY -->
    <main class="gallery">
      <div
        class="track"
        :style="{ transform: `translateX(-${current * 100}%)` }"
      >
        <article class="slide" v-for="item in items" :key="item.id">
          <!-- Background image: grayscale + low opacity -->
          <img v-if="item.img" class="slide-img" :src="item.img" :alt="item.name" />

          <!-- Decorative background character -->
          <div class="bg-char" aria-hidden="true">{{ item.char }}</div>

          <!-- Content -->
          <div class="content">
            <span class="category">{{ item.category }}</span>
            <h2 class="name">{{ item.name }}</h2>
            <p class="name-en">{{ item.nameEn }}</p>
            <p class="period">{{ item.period }}</p>
            <div class="divider" />
            <p class="desc">{{ item.desc }}</p>
          </div>

          <!-- Side decoration -->
          <div class="side-deco" aria-hidden="true">
            <span>{{ item.char }}</span>
          </div>
        </article>
      </div>
    </main>

    <!-- NAVIGATION -->
    <footer class="nav">
      <button class="nav-btn" @click="prev" :disabled="current === 0" aria-label="이전">←</button>

      <div class="dots" role="tablist">
        <button
          v-for="(item, i) in items"
          :key="i"
          :class="['dot', { active: i === current }]"
          @click="current = i"
          :aria-label="item.name"
          role="tab"
        />
      </div>

      <button class="nav-btn" @click="next" :disabled="current === items.length - 1" aria-label="다음">→</button>
    </footer>

  </div>

  <!-- LOGIN MODAL -->
  <Transition name="modal">
    <div v-if="showModal" class="modal-overlay">
      <div class="modal">
        <button class="modal-close" @click="closeModal" aria-label="닫기">×</button>

        <div class="modal-header">
          <p class="modal-sub">Korean Cultural Heritage</p>
          <h2 class="modal-title">
            {{ formMode === 'login' ? '로그인' : formMode === 'signup' ? '회원가입' : '비밀번호 찾기' }}
          </h2>
        </div>

        <!-- 로그인 -->
        <form v-if="formMode === 'login'" class="modal-form" @submit.prevent="handleLogin">
          <div class="field">
            <label>이메일</label>
            <input v-model="loginEmail" type="email" placeholder="email@example.com" />
          </div>
          <div class="field">
            <label>비밀번호</label>
            <input v-model="loginPassword" type="password" placeholder="••••••••" />
          </div>
          <p v-if="loginError" class="login-error">{{ loginError }}</p>
          <button class="btn-primary" type="submit">로그인</button>
          <div class="modal-links">
            <button type="button" @click="switchTo('forgot')">비밀번호를 잊으셨나요?</button>
            <button type="button" @click="switchTo('signup')">회원가입</button>
          </div>
        </form>

        <!-- 회원가입 -->
        <form v-else-if="formMode === 'signup'" class="modal-form" @submit.prevent>
          <div class="field">
            <label>이름</label>
            <input v-model="signupName" type="text" placeholder="홍길동" />
          </div>
          <div class="field">
            <label>이메일</label>
            <input v-model="signupEmail" type="email" placeholder="email@example.com" />
          </div>
          <div class="field">
            <label>비밀번호</label>
            <input v-model="signupPassword" type="password" placeholder="••••••••" />
          </div>
          <div class="field">
            <label>비밀번호 확인</label>
            <input v-model="signupConfirm" type="password" placeholder="••••••••" />
          </div>
          <button class="btn-primary" type="submit">회원가입</button>
          <div class="modal-links">
            <button type="button" @click="switchTo('login')">이미 계정이 있으신가요?</button>
          </div>
        </form>

        <!-- 비밀번호 찾기 -->
        <form v-else class="modal-form" @submit.prevent>
          <p class="modal-desc">가입한 이메일 주소를 입력하시면 비밀번호 재설정 링크를 보내드립니다.</p>
          <div class="field">
            <label>이메일</label>
            <input v-model="forgotEmail" type="email" placeholder="email@example.com" />
          </div>
          <button class="btn-primary" type="submit">재설정 링크 보내기</button>
          <div class="modal-links">
            <button type="button" @click="switchTo('login')">로그인으로 돌아가기</button>
          </div>
        </form>

      </div>
    </div>
  </Transition>

</template>

<style>
/* ── Reset ── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html, body, #app {
  height: 100%;
}

body {
  background: #0c0c0c;
  color: #ede9e0;
  font-family: 'Noto Serif KR', 'Batang', Georgia, serif;
  -webkit-font-smoothing: antialiased;
  overflow: hidden;
}

/* ── App shell ── */
.app {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

/* ── Header ── */
.header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 2rem 3.5rem 1.5rem;
  border-bottom: 1px solid #1e1e1e;
  flex-shrink: 0;
}

.header-en {
  font-size: 0.625rem;
  letter-spacing: 0.4em;
  text-transform: uppercase;
  color: #3a3a3a;
  margin-bottom: 0.35rem;
  font-weight: 300;
}

.header-ko {
  font-size: 1.25rem;
  font-weight: 200;
  letter-spacing: 0.12em;
  color: #ede9e0;
}

.counter {
  font-size: 0.75rem;
  color: #3a3a3a;
  letter-spacing: 0.15em;
}

.counter em {
  font-style: normal;
  color: #ede9e0;
  font-size: 1rem;
  font-weight: 300;
}

/* ── Gallery ── */
.gallery {
  flex: 1;
  overflow: hidden;
  position: relative;
}

.track {
  display: flex;
  height: 100%;
  transition: transform 0.75s cubic-bezier(0.76, 0, 0.24, 1);
  will-change: transform;
}

.slide {
  min-width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  align-items: flex-end;
  padding: 3rem 3.5rem;
  overflow: hidden;
}

/* Background image: full-cover, grayscale, faded */
.slide-img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  filter: grayscale(100%);
  opacity: 0.13;
  pointer-events: none;
  user-select: none;
}

/* Large ghostly character (backdrop) */
.bg-char {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -52%);
  font-size: clamp(14rem, 38vw, 32rem);
  color: #ede9e0;
  opacity: 0.035;
  line-height: 1;
  user-select: none;
  pointer-events: none;
  font-weight: 600;
  letter-spacing: 0;
}

/* Right-side vertical decoration */
.side-deco {
  position: absolute;
  right: 3.5rem;
  bottom: 3rem;
  writing-mode: vertical-rl;
  text-orientation: mixed;
  font-size: 0.65rem;
  letter-spacing: 0.4em;
  color: #252525;
  user-select: none;
  pointer-events: none;
}

/* Slide text content */
.content {
  position: relative;
  z-index: 1;
  max-width: 560px;
}

.category {
  display: block;
  font-size: 0.6rem;
  letter-spacing: 0.4em;
  color: #3a3a3a;
  margin-bottom: 1.1rem;
  text-transform: uppercase;
}

.name {
  font-size: clamp(2.8rem, 7vw, 5rem);
  font-weight: 200;
  letter-spacing: 0.06em;
  line-height: 1.05;
  color: #ede9e0;
}

.name-en {
  font-size: 0.8rem;
  color: #555;
  letter-spacing: 0.18em;
  margin-top: 0.6rem;
  font-weight: 300;
}

.period {
  font-size: 0.7rem;
  color: #333;
  letter-spacing: 0.22em;
  margin-top: 0.3rem;
}

.divider {
  width: 48px;
  height: 1px;
  background: #252525;
  margin: 1.4rem 0;
}

.desc {
  font-size: 0.875rem;
  line-height: 2;
  color: #5a5a5a;
  max-width: 460px;
  word-break: keep-all;
  font-weight: 300;
}

/* ── Navigation ── */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem 3.5rem 1.75rem;
  border-top: 1px solid #161616;
  flex-shrink: 0;
}

.nav-btn {
  background: transparent;
  border: 1px solid #222;
  color: #555;
  width: 42px;
  height: 42px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 1rem;
  transition: border-color 0.2s, color 0.2s;
  font-family: inherit;
  letter-spacing: 0;
  line-height: 1;
}

.nav-btn:hover:not(:disabled) {
  border-color: #ede9e0;
  color: #ede9e0;
}

.nav-btn:disabled {
  opacity: 0.15;
  cursor: not-allowed;
}

.dots {
  display: flex;
  gap: 8px;
  align-items: center;
}

.dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: #222;
  border: none;
  cursor: pointer;
  transition: background 0.3s, width 0.3s, border-radius 0.3s;
  padding: 0;
}

.dot.active {
  background: #ede9e0;
  width: 28px;
  border-radius: 2px;
}

.dot:hover:not(.active) {
  background: #444;
}

/* ── Modal ── */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.82);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  backdrop-filter: blur(6px);
}

.modal {
  background: #1a1a1a;
  border: 1px solid #2e2e2e;
  width: 100%;
  max-width: 420px;
  padding: 3rem;
  position: relative;
}

.modal-close {
  position: absolute;
  top: 1.25rem;
  right: 1.5rem;
  background: transparent;
  border: none;
  color: #666;
  font-size: 1.5rem;
  cursor: pointer;
  line-height: 1;
  transition: color 0.2s;
  font-family: inherit;
}

.modal-close:hover { color: #ede9e0; }

.modal-header { margin-bottom: 2rem; }

.modal-sub {
  font-size: 0.6rem;
  letter-spacing: 0.4em;
  text-transform: uppercase;
  color: #777;
  margin-bottom: 0.5rem;
}

.modal-title {
  font-size: 1.75rem;
  font-weight: 200;
  letter-spacing: 0.1em;
  color: #ede9e0;
}

.modal-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.modal-desc {
  font-size: 0.8rem;
  color: #888;
  line-height: 1.9;
  letter-spacing: 0.03em;
  word-break: keep-all;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.field label {
  font-size: 0.6rem;
  letter-spacing: 0.35em;
  color: #888;
  text-transform: uppercase;
}

.field input {
  background: #111;
  border: 1px solid #333;
  color: #ede9e0;
  padding: 0.75rem 1rem;
  font-family: inherit;
  font-size: 0.875rem;
  outline: none;
  transition: border-color 0.2s;
}

.field input::placeholder { color: #444; }
.field input:focus { border-color: #888; }

.login-error {
  font-size: 0.72rem;
  color: #c97b7b;
  letter-spacing: 0.05em;
  margin-top: -0.25rem;
}

.btn-primary {
  background: transparent;
  border: 1px solid #555;
  color: #ede9e0;
  padding: 0.875rem;
  font-family: inherit;
  font-size: 0.7rem;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  cursor: pointer;
  transition: border-color 0.25s, background 0.25s, color 0.25s;
  margin-top: 0.25rem;
}

.btn-primary:hover {
  border-color: #ede9e0;
  background: #ede9e0;
  color: #0c0c0c;
}

.modal-links {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}

.modal-links button {
  background: transparent;
  border: none;
  color: #777;
  font-family: inherit;
  font-size: 0.68rem;
  letter-spacing: 0.12em;
  cursor: pointer;
  transition: color 0.2s;
  padding: 0;
}

.modal-links button:hover { color: #ede9e0; }

/* Modal transition */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.35s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-active .modal,
.modal-leave-active .modal {
  transition: transform 0.35s ease, opacity 0.35s ease;
}

.modal-enter-from .modal,
.modal-leave-to .modal {
  transform: translateY(-12px);
  opacity: 0;
}
</style>
