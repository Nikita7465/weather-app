<template>
  <div class="wrap">
    <div class="sky"></div>
    <div class="sunlight"></div>
    <div class="sun"></div>
    <div class="mountains"></div>
    <div class="clouds"></div>
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
  background: #6cdcff;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.sunlight {
  background: linear-gradient(to right, #ffe50030, #00000020);
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.sun {
  background: url(/src/assets/sunrise-sun.png) top left no-repeat;
  background-size: contain;
  width: 500%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
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

.clouds {
  background: url(/src/assets/clouds.png) center no-repeat;
  background-size: 100% 100%;
  width: 200%;
  height: 20%;
  position: absolute;
  top: 0;
  right: -20%;
  animation: cloudsMove 1000s linear infinite;
}

@media (max-width: 1024px) {
  .clouds {
    width: 250%;
  }
}

@media (max-width: 53.75rem) {
  .mountains {
    width: 150%;
  }
  .clouds {
    width: 300%;
  }
}

@media (max-width: 41.75rem) {
  .clouds {
    width: 400%;
  }
}

@media (max-width: 568px) {
  .mountains {
    width: 200%;
  }
  .clouds {
    width: 450%;
  }
}

@media (max-width: 468px) {
  .clouds {
    width: 500%;
  }
}

@media (max-width: 425px) {
  .mountains {
    width: 300%;
  }
}

@media (max-width: 380px) {
  .clouds {
    width: 600%;
  }
}

@media (max-width: 320px) {
  .mountains {
    width: 350%;
  }
  .clouds {
    width: 700%;
  }
}

@keyframes cloudsMove {
  to {
    transform: translateX(33.3%);
  }
}
</style>
