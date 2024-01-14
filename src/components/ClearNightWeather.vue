<template>
  <div class="wrap">
    <div class="sky"></div>
    <div class="stars"></div>
    <div class="mountains"></div>
    <div class="blackout"></div>
    <div class="moon"></div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.wrap {
  width: 100vw;
  height: 100dvh;
  overflow: hidden;
  position: fixed;
  top: 0;
  left: 0;
  z-index: -10;
}

.sky {
  background: #000000;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.stars {
  background: url(/src/assets/stars.png) center no-repeat;
  background-size: cover;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.mountains {
  background: url(/src/assets/mountains.png) center no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 55%;
  transform: translateX(-50%);
  position: absolute;
  left: 50%;
  right: 0;
  bottom: 0;
}

.blackout {
  background: #00000088;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.moon {
  background: url(/src/assets/moon.png) center top no-repeat;
  background-size: contain;
  width: 500%;
  height: 100%;
  transform: translateX(-50%);
  position: absolute;
  top: 0;
  left: 50%;
}

@media (max-width: 860px) {
  .mountains {
    width: 150%;
  }
}

@media (max-width: 568px) {
  .mountains {
    width: 200%;
  }
}

@media (max-width: 425px) {
  .mountains {
    width: 300%;
  }
}

@media (max-width: 320px) {
  .mountains {
    width: 350%;
  }
}
</style>
