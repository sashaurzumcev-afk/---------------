<template>
  <div class="container" id="mainContainer">
    <div class="lili-container" @click="handleLiliClick">
      <!--<img src="/lili.gif" alt="LILI">-->
    </div>

    <h1>LILI<span class="accent">.</span></h1>
    <div class="subtitle">AI Security Defender</div>
    <p class="tagline">
      Интеллектуальная защита для ваших данных.<br>
      Анализ угроз, обнаружение атак, рекомендации в реальном времени.
    </p>

    <div class="click-hint">✦ нажми на LILI, чтобы начать ✦</div>

    <div class="footer">
      <span>⚡ v2.0 · Сделано с ❤️ для защиты данных</span>
    </div>
  </div>
</template>

<script setup>
const handleLiliClick = () => {
  const container = document.getElementById('mainContainer');
  if (container) container.classList.add('fade-out');

  setTimeout(() => {
    navigateTo('/chat');
  }, 800);
};
</script>

<style scoped>
.container {
  text-align: center;
  max-width: 800px;
  padding: 20px;
  z-index: 10;
  position: relative;
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.container.fade-out {
  opacity: 0;
  transform: scale(0.9) translateY(-20px);
  pointer-events: none;
}

.lili-container {
  width: 220px;
  height: 220px;
  margin: 0 auto 25px;
  cursor: pointer;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 0 60px rgba(123, 47, 252, 0.3);
  transition: 0.3s;
  animation: float 3s ease-in-out infinite;
  border: 3px solid rgba(123, 47, 252, 0.2);
}

.lili-container:hover {
  transform: scale(1.05);
  box-shadow: 0 0 80px rgba(123, 47, 252, 0.5);
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.lili-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

h1 {
  font-family: 'Orbitron', sans-serif;
  font-weight: 900;
  font-size: 4rem;
  color: #fff;
  margin-bottom: 5px;
  letter-spacing: 4px;
  text-shadow: 0 0 40px rgba(123, 47, 252, 0.15);
}

h1 .accent {
  color: #7b2ffc;
}

.subtitle {
  font-size: 1.2rem;
  color: #808090;
  margin-bottom: 10px;
  letter-spacing: 6px;
  text-transform: uppercase;
  font-weight: 300;
}

.tagline {
  font-size: 1.1rem;
  color: #a0a0b0;
  margin-bottom: 30px;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
  line-height: 1.6;
}

.click-hint {
  color: #505060;
  font-size: 0.8rem;
  letter-spacing: 2px;
  animation: pulseText 2s infinite;
}

@keyframes pulseText {
  0%, 100% { opacity: 0.5; }
  50% { opacity: 1; }
}

.footer {
  margin-top: 40px;
  font-size: 0.75rem;
  color: #505060;
  letter-spacing: 2px;
}

@media (max-width: 600px) {
  h1 { font-size: 2.5rem; }
  .subtitle { font-size: 0.8rem; letter-spacing: 3px; }
  .tagline { font-size: 0.95rem; }
  .lili-container { width: 150px; height: 150px; }
}
</style>