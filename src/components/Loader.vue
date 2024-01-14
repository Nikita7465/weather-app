<template>
  <div class="blackout">
    <div class="loader">
      <div class="sun"></div>
      <div class="moon"></div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.blackout {
  background: #00000099;
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 10;
}

.loader {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  animation: loaderAnim 1s ease-in-out infinite;
}

.sun {
  background: url(/public/sun-loader.svg) center no-repeat;
  background-size: contain;
  width: 3.4375rem;
  height: 3.4375rem;
}
.moon {
  background: url(/public/moon-loader.svg) center no-repeat;
  background-size: contain;
  width: 3.125rem;
  height: 3.125rem;
}

@keyframes loaderAnim {
  50% {
    transform: rotate(180deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
