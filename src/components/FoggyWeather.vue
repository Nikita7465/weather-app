<template>
  <div class="wrap">
    <div class="sky"></div>
    <div class="mountains"></div>
    <div class="fog"></div>
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
  background: #9fcddc;
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

.fog {
  background: linear-gradient(to top, #eaeaeac0, #eaeaea00),
    linear-gradient(to bottom, #eaeaea10, #eaeaea00);
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
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
