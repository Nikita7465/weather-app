<template>
  <div class="wrap">
    <div class="sky"></div>
    <div class="mountains"></div>
    <div class="snowflakes-back"></div>
    <div class="snowflakes-front"></div>
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
  background: #a5e4f8;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.mountains {
  background: url(/src/assets/snowy-mountains.png) center no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 55%;
  transform: translateX(-50%);
  position: absolute;
  left: 50%;
  right: 0;
  bottom: 0;
}

.snowflakes-back {
  background: url(/src/assets/snowflakes-back.png) center no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 200%;
  position: absolute;
  left: 0;
  bottom: 0;
  animation: back-snowfall 20s linear infinite;
}

.snowflakes-front {
  background: url(/src/assets/snowflakes-front.png) center no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 200%;
  position: absolute;
  left: 0;
  bottom: 0;
  animation: front-snowfall 10s linear infinite;
}

.clouds {
  background: url(/src/assets/snow-clouds.png) center no-repeat;
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
    width: 300%;
  }
}

@media (max-width: 860px) {
  .mountains {
    width: 150%;
  }
  .clouds {
    width: 450%;
  }

  .snowflakes-back,
  .snowflakes-front {
    width: 150%;
  }
}

@media (max-width: 668px) {
  .clouds {
    width: 500%;
  }
}

@media (max-width: 568px) {
  .mountains {
    width: 200%;
  }
  .clouds {
    width: 650%;
  }

  .snowflakes-back,
  .snowflakes-front {
    width: 200%;
  }
}

@media (max-width: 468px) {
  .clouds {
    width: 700%;
  }
}

@media (max-width: 425px) {
  .mountains {
    width: 300%;
  }
}

@media (max-width: 380px) {
  .clouds {
    width: 750%;
  }

  .snowflakes-back,
  .snowflakes-front {
    width: 250%;
  }
}

@media (max-width: 320px) {
  .mountains {
    width: 350%;
  }
  .clouds {
    width: 800%;
  }
}

@keyframes cloudsMove {
  to {
    transform: translateX(33.3%);
  }
}

@keyframes back-snowfall {
  to {
    transform: translateY(50%);
  }
}

@keyframes front-snowfall {
  to {
    transform: translateY(50%);
  }
}
</style>
