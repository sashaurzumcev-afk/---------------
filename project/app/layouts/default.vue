<template>
  <div class="layout-wrapper">
    <div class="grid-bg"></div>
    <div class="scanline"></div>
    <slot />
  </div>
</template>

<style lang="scss">
import
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: #05050a;
  color: #e0e0e0;
  font-family: 'Rajdhani', sans-serif;
  height: 100vh;
  @include flex;
  overflow: hidden;
  position: relative;
}
</style>


<style scoped lang="scss">
.layout-wrapper {
  position: relative;
  min-height: 100vh;
  width: 100%;
  @include flex;
  overflow: hidden;
}

.grid-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  background-image:
    linear-gradient(rgba(0, 212, 255, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 212, 255, 0.03) 1px, transparent 1px);
  background-size: 60px 60px;
  animation: gridMove 20s linear infinite;
}

@keyframes gridMove {
  0% { transform: translate(0, 0); }
  100% { transform: translate(60px, 60px); }
}

.scanline {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(0, 212, 255, 0.12), transparent);
  z-index: 0;
  animation: scan 6s linear infinite;
}

@keyframes scan {
  0% { top: 0; }
  100% { top: 100%; }
}
</style>