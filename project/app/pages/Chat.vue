<template>
  <div class="main-wrapper">
    <!-- Боковая панель с LILI -->
    <div class="lili-side">
      <!--<img src="/lili-def_orig.png" alt="LILI">-->
    </div>
    <!-- Контейнер чата -->
    <div class="chat-container">
      <div class="chat-header">
        <a class="back" @click.prevent="goHome">
          <i class="fas fa-arrow-left"></i> Назад
        </a>
        <div class="header-center">
          <h2><span class="status-dot"></span> LILI <span class="pink">♥</span> Chat</h2>
        </div>
        <div class="controls">
          <button class="voice-btn" :class="{ active: voiceEnabled }" @click="toggleVoice" title="Голос">
            <i :class="voiceEnabled ? 'fas fa-volume-up' : 'fas fa-volume-mute'"></i>
          </button>
        </div>
      </div>
      <!-- Эмоции -->
      <div class="emotion-panel">
        <button
          v-for="emo in emotions"
          :key="emo.value"
          class="emo-btn"
          :class="{ active: currentEmotion === emo.value }"
          @click="setEmotion(emo.value)"
        >
          {{ emo.label }}
        </button>
      </div>
      <!-- Сообщения -->
      <div class="messages" ref="messagesContainer">
        <div
          v-for="(msg, idx) in messages"
          :key="idx"
          class="msg"
          :class="msg.sender"
        >
          {{ msg.text }}
        </div>
      </div>
      <!-- Индикатор печати -->
      <div class="typing" v-show="isTyping">LILI думает...</div>
      <!-- Поле ввода -->
      <div class="input-area">
        <input
          type="text"
          v-model="userInput"
          placeholder="Напиши что-нибудь..."
          @keyup.enter="sendMessage"
          autocomplete="off"
        />
        <button class="btn-send" @click="sendMessage">
          <i class="fas fa-paper-plane"></i>
        </button>
      </div>
    </div>
  </div>
</template>



<style scoped lang="scss">
/* --- Все стили из исходного HTML, адаптированы под scoped --- */
:root {
  --bg-color: #05050a;
  --accent-color: #00d4ff;
  --accent-glow: rgba(0, 212, 255, 0.3);
  --pink-color: #ff6b9d;
  --text-primary: #e0e0e0;
  --text-secondary: #808090;
  --panel-bg: rgba(15, 15, 25, 0.88);
}

.main-wrapper {
  @include flex;
  gap: 30px;
  width: 100%;
  max-width: 1100px;
  padding: 20px;
  z-index: 10;
  height: 90vh;
}

.lili-side {
  flex-shrink: 0;
  width: 280px;
  height: 400px;
  @include flex;
  animation: float 3s ease-in-out infinite;
  filter: drop-shadow(0 0 40px rgba(123, 47, 252, 0.25));
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-12px); }
}

.lili-side img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  display: block;
}

.chat-container {
  flex: 1;
  min-width: 0;
  height: 100%;
  max-height: 90vh;
  background: var(--panel-bg);
  border: 1px solid rgba(0, 212, 255, 0.1);
  border-radius: 24px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 60px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(12px);
  animation: slideUp 0.6s ease;
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

.chat-header {
  padding: 12px 24px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-shrink: 0;
}

.chat-header .back {
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 0.8rem;
  transition: 0.3s;
  display: flex;
  align-items: center;
  gap: 6px;
  cursor: pointer;
}

.chat-header .back:hover {
  color: var(--accent-color);
}

.header-center h2 {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.9rem;
  color: var(--accent-color);
  letter-spacing: 2px;
}

.header-center h2 .pink {
  color: var(--pink-color);
}

.status-dot {
  width: 8px;
  height: 8px;
  background: #00ff88;
  border-radius: 50%;
  display: inline-block;
  margin-right: 6px;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.3; }
}

.voice-btn {
  background: transparent;
  border: none;
  color: var(--text-secondary);
  cursor: pointer;
  font-size: 1.1rem;
  transition: 0.3s;
  padding: 4px 8px;
  border-radius: 8px;
}

.voice-btn:hover {
  color: var(--pink-color);
}
.voice-btn.active {
  color: var(--pink-color);
}

.emotion-panel {
  display: flex;
  gap: 8px;
  padding: 10px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.04);
  justify-content: center;
  flex-shrink: 0;
}

.emo-btn {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.06);
  color: white;
  padding: 4px 14px;
  border-radius: 20px;
  cursor: pointer;
  transition: 0.3s;
  font-size: 1rem;
}

.emo-btn:hover,
.emo-btn.active {
  background: var(--accent-glow);
  border-color: var(--accent-color);
}

.messages {
  flex: 1;
  overflow-y: auto;
  padding: 20px 24px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.messages::-webkit-scrollbar {
  width: 4px;
}
.messages::-webkit-scrollbar-thumb {
  background: var(--accent-glow);
  border-radius: 10px;
}

.msg {
  max-width: 85%;
  padding: 12px 18px;
  border-radius: 14px;
  font-size: 0.95rem;
  line-height: 1.5;
  animation: fadeIn 0.3s ease;
  word-wrap: break-word;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.msg.user {
  background: rgba(0, 212, 255, 0.12);
  align-self: flex-end;
  border-right: 3px solid var(--accent-color);
}

.msg.ai {
  background: rgba(255, 255, 255, 0.04);
  align-self: flex-start;
  border-left: 3px solid var(--pink-color);
  font-family: 'Fira Code', monospace;
  font-size: 0.9rem;
}

.input-area {
  padding: 14px 20px;
  display: flex;
  gap: 10px;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  background: rgba(0, 0, 0, 0.2);
  border-radius: 0 0 24px 24px;
  flex-shrink: 0;
}

.input-area input {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.06);
  color: white;
  padding: 12px 16px;
  border-radius: 10px;
  outline: none;
  font-family: 'Rajdhani', sans-serif;
  font-size: 1rem;
  transition: 0.3s;
}

.input-area input:focus {
  border-color: var(--accent-color);
}

.btn-send {
  background: var(--accent-color);
  border: none;
  padding: 0 24px;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
  color: #000;
  transition: 0.3s;
}

.btn-send:hover {
  transform: scale(1.05);
  box-shadow: 0 0 25px var(--accent-color);
}

.typing {
  font-size: 0.8rem;
  color: var(--text-secondary);
  padding: 0 24px 8px;
  font-style: italic;
  flex-shrink: 0;
}

@media (max-width: 768px) {
  .main-wrapper {
    flex-direction: column;
    gap: 10px;
    height: 100vh;
    padding: 10px;
  }
  .lili-side {
    width: 120px;
    height: 150px;
    order: -1;
  }
  .chat-container {
    height: 70vh;
    max-height: 70vh;
  }
  .msg {
    max-width: 95%;
    font-size: 0.85rem;
  }
  .chat-header h2 {
    font-size: 0.7rem;
  }
}

@media (max-width: 480px) {
  .lili-side {
    width: 80px;
    height: 100px;
  }
  .chat-container {
    height: 65vh;
    max-height: 65vh;
  }
  .chat-header .back {
    font-size: 0.6rem;
  }
  .chat-header h2 {
    font-size: 0.6rem;
  }
}
</style>

<script setup>
import { ref, onMounted, nextTick } from 'vue'
import { navigateTo } from '#app'

// ---------- Конфигурация API ----------
const API_URL = 'http://localhost:8000/generate'
const API_KEY = 'sd_secure_2026_key'

// ---------- ElevenLabs ----------
const ELEVENLABS_API_KEY = 'sk_b8535c56f6f620ff707ad47ed541d78100e2a1b6eff4ce60'
const ELEVENLABS_VOICE_ID = 'DM2QdfBtii3EkC6ChAJa'

// ---------- Реактивные данные ----------
const messages = ref([
  { text: '🌸 Привет! Я LILI — твой милый защитник. Чем могу помочь?', sender: 'ai' }
])
const userInput = ref('')
const isTyping = ref(false)
const voiceEnabled = ref(true)
const currentEmotion = ref('neutral')

const emotions = [
  { value: 'neutral', label: '😐' },
  { value: 'happy', label: '😊' },
  { value: 'angry', label: '😡' }
]

const messagesContainer = ref(null)

// ---------- Методы ----------
function goHome() {
  navigateTo('/')
}

function setEmotion(emo) {
  currentEmotion.value = emo
}

// ---------- Голос ----------
function fixRussianStress(text) {
  const stressMap = {
    'привет': 'прив\'ет',
    'Лили': 'Л\'или',
    'меня': 'мен\'я',
    'зовут': 'зов\'ут',
    'кибербезопасность': 'кибербезоп\'асность',
    'уязвимость': 'уязв\'имость',
    'рекомендация': 'рекоменд\'ация',
    'митигация': 'митиг\'ация',
    'атака': 'ат\'ака',
    'защита': 'защ\'ита',
    'сервер': 'с\'ервер',
    'данные': 'д\'анные',
    'пароль': 'пар\'оль',
    'вход': 'вх\'од',
    'система': 'сист\'ема',
    'безопасность': 'безоп\'асность',
    'помощь': 'п\'омощь',
    'ответ': 'отв\'ет',
    'спасибо': 'спас\'ибо',
    'пожалуйста': 'пож\'алуйста'
  }
  let result = text
  for (const [word, fixed] of Object.entries(stressMap)) {
    const regex = new RegExp(`\\b${word}\\b`, 'gi')
    result = result.replace(regex, fixed)
  }
  return result
}

async function speak(text) {
  if (!voiceEnabled.value) return
  const textWithStress = fixRussianStress(text)

  try {
    const response = await fetch(`https://api.elevenlabs.io/v1/text-to-speech/${ELEVENLABS_VOICE_ID}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'xi-api-key': ELEVENLABS_API_KEY
      },
      body: JSON.stringify({
        text: textWithStress,
        model_id: 'eleven_multilingual_v2',
        language_code: 'ru',
        voice_settings: {
          stability: 0.35,
          similarity_boost: 0.65,
          style: 0.45,
          use_speaker_boost: true
        }
      })
    })

    if (!response.ok) throw new Error('ElevenLabs error')
    const audioBlob = await response.blob()
    const audioUrl = URL.createObjectURL(audioBlob)
    const audio = new Audio(audioUrl)
    audio.play()
  } catch (e) {
    console.warn('ElevenLabs failed, fallback:', e)
    fallbackSpeak(text)
  }
}

function fallbackSpeak(text) {
  const utterance = new SpeechSynthesisUtterance(text)
  utterance.lang = 'ru-RU'
  utterance.rate = 0.9
  utterance.pitch = 1.2
  utterance.volume = 1

  const voices = window.speechSynthesis.getVoices()
  const femaleVoice = voices.find(v => v.lang.startsWith('ru') && v.name.includes('Female'))
  if (femaleVoice) utterance.voice = femaleVoice

  window.speechSynthesis.speak(utterance)
}

function toggleVoice() {
  voiceEnabled.value = !voiceEnabled.value
}

// ---------- Отправка сообщения ----------
async function sendMessage() {
  const text = userInput.value.trim()
  if (!text || isTyping.value) return

  // Добавляем сообщение пользователя
  messages.value.push({ text, sender: 'user' })
  userInput.value = ''
  isTyping.value = true

  // Прокрутка вниз
  await nextTick()
  scrollToBottom()

  try {
    const res = await fetch(API_URL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-API-Key': API_KEY
      },
      body: JSON.stringify({ prompt: text, emotion: currentEmotion.value })
    })

    if (!res.ok) {
      const err = await res.json()
      throw new Error(err.detail || 'Ошибка сервера')
    }

    const data = await res.json()
    const reply = data.response || '✅ Запрос обработан.'
    messages.value.push({ text: reply, sender: 'ai' })
    speak(reply)
  } catch (e) {
    messages.value.push({
      text: `❌ Ошибка: ${e.message || 'Сервер недоступен. Запустите backend/server.py'}`,
      sender: 'ai'
    })
  } finally {
    isTyping.value = false
    await nextTick()
    scrollToBottom()
  }
}

function scrollToBottom() {
  if (messagesContainer.value) {
    messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
  }
}

// ---------- Жизненный цикл ----------
onMounted(() => {
  // Получаем голоса для резерва
  window.speechSynthesis.getVoices()
  window.speechSynthesis.onvoiceschanged = () => window.speechSynthesis.getVoices()

  console.log('🌸 LILI Chat loaded!')
  setTimeout(() => {
    speak('Привет! Я Лили, твой милый защитник.')
  }, 1000)
})
</script>