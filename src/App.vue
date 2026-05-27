<template>
  <div class="relative flex flex-col h-full text-white overflow-hidden app-bg">

    <!-- Animated background blobs -->
    <div class="bg-blob bg-blob-1"></div>
    <div class="bg-blob bg-blob-2"></div>
    <div class="bg-blob bg-blob-3"></div>

    <!-- STATIC HOLDING PAGE -->
    <div class="relative z-10 flex-1 flex flex-col items-center justify-center gap-8 px-5 text-center">
      <h1 class="title-glow font-extrabold leading-tight">
        <span class="block bg-clip-text text-transparent bg-gradient-to-r from-pink-400 via-fuchsia-400 to-purple-400 text-5xl sm:text-6xl">
          Mascha &amp; Cedi
        </span>
      </h1>

      <div class="bubble">
        <img src="/images/xx.jpeg" alt="Mascha & Cedi" />
      </div>

      <p class="text-white/60 text-lg font-medium tracking-wide">
        Current State: Something
      </p>
    </div>

  </div>
</template>

<script setup>
</script>

<style scoped>
.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: opacity 0.4s ease, transform 0.4s ease;
}
.slide-fade-enter-from {
  opacity: 0;
  transform: translateX(30px);
}
.slide-fade-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

/* ---------- Background ---------- */
.app-bg {
  background:
    radial-gradient(ellipse at top, #2a0a3a 0%, #0a0612 55%, #050208 100%);
}

.bg-blob {
  position: absolute;
  border-radius: 9999px;
  filter: blur(80px);
  opacity: 0.55;
  pointer-events: none;
  z-index: 0;
}
.bg-blob-1 {
  width: 340px; height: 340px;
  background: #ec4899;
  top: -80px; left: -80px;
  animation: float 9s ease-in-out infinite;
}
.bg-blob-2 {
  width: 380px; height: 380px;
  background: #8b5cf6;
  bottom: -100px; right: -100px;
  animation: float 11s ease-in-out infinite reverse;
}
.bg-blob-3 {
  width: 260px; height: 260px;
  background: #06b6d4;
  top: 40%; left: 50%;
  transform: translate(-50%, -50%);
  opacity: 0.35;
  animation: float 13s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translate(0, 0) scale(1); }
  50%      { transform: translate(20px, -30px) scale(1.08); }
}

/* ---------- Title ---------- */
.title-glow {
  text-shadow: 0 0 25px rgba(236, 72, 153, 0.35);
}

/* ---------- Bubbles ---------- */
.bubble {
  flex: 0 0 auto;
  width: min(60vw, 260px);
  max-height: 40vh;
  border-radius: 28px;
  overflow: hidden;
  border: 4px solid rgba(255, 255, 255, 0.92);
  box-shadow:
    0 0 0 4px rgba(236, 72, 153, 0.35),
    0 20px 60px -10px rgba(236, 72, 153, 0.6),
    inset 0 0 30px rgba(0, 0, 0, 0.25);
  background:
    radial-gradient(circle at 30% 30%, #2a0a3a 0%, #0a0612 100%);
}
.bubble img {
  width: 100%;
  height: 100%;
  max-height: 40vh;
  object-fit: cover;
  object-position: center center;
  padding: 0;
  display: block;
}
.bubble-left {
  animation: bob 5s ease-in-out infinite;
  margin-top: -14px;
}
.bubble-right {
  box-shadow:
    0 0 0 4px rgba(139, 92, 246, 0.4),
    0 20px 60px -10px rgba(139, 92, 246, 0.6),
    inset 0 0 30px rgba(0, 0, 0, 0.25);
  animation: bob 6s ease-in-out infinite reverse;
}

@keyframes bob {
  0%, 100% { transform: translateY(0) rotate(-2deg); }
  50%      { transform: translateY(-10px) rotate(2deg); }
}

.heart-pulse {
  font-size: clamp(1.75rem, 6vw, 2.5rem);
  filter: drop-shadow(0 0 12px rgba(236, 72, 153, 0.8));
  animation: heartbeat 1.4s ease-in-out infinite;
  flex: 0 0 auto;
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  25%      { transform: scale(1.25); }
  50%      { transform: scale(1); }
  75%      { transform: scale(1.15); }
}

/* ---------- Start button ---------- */
.start-btn {
  background: linear-gradient(135deg, #ec4899 0%, #a855f7 50%, #6366f1 100%);
  background-size: 200% 200%;
  box-shadow:
    0 10px 40px -10px rgba(236, 72, 153, 0.7),
    0 0 0 2px rgba(255, 255, 255, 0.1) inset;
  animation: shimmer 4s ease infinite, pulse-btn 2.2s ease-in-out infinite;
}
@keyframes shimmer {
  0%, 100% { background-position: 0% 50%; }
  50%      { background-position: 100% 50%; }
}
@keyframes pulse-btn {
  0%, 100% { box-shadow: 0 10px 40px -10px rgba(236, 72, 153, 0.7), 0 0 0 2px rgba(255,255,255,0.1) inset; }
  50%      { box-shadow: 0 10px 50px -5px rgba(168, 85, 247, 0.9), 0 0 0 2px rgba(255,255,255,0.2) inset; }
}
</style>

