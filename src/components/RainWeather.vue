<template>
  <div class="wrap">
    <div class="sky"></div>
    <div class="mountains"></div>
    <div class="rain"></div>
    <div class="clouds"></div>
    <!-- <audio :src="audioSource" autoplay loop></audio> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      audioSource: require("/src/assets/rain.mp3"),
    };
  },
};
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
  background: #919c9f;
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

.rain {
  background: url(/src/assets/raindrops.png) center no-repeat;
  background-size: cover;
  width: 200%;
  height: 200%;
  position: absolute;
  left: 0;
  bottom: 0;
  animation: rainfall 1s linear infinite;
}

.clouds {
  background: url(/src/assets/rain-clouds.png) center no-repeat;
  background-size: 100% 100%;
  width: 200%;
  height: 20%;
  position: absolute;
  top: 0;
  right: -20%;
  animation: cloudsMove 100s linear infinite;
}

@media (max-width: 1024px) {
  .clouds {
    width: 300%;
  }

  .rain {
    width: 250%;
  }
}

@media (max-width: 860px) {
  .mountains {
    width: 150%;
  }
  .clouds {
    width: 450%;
  }

  .rain {
    width: 300%;
  }
}

@media (max-width: 668px) {
  .clouds {
    width: 500%;
  }

  .rain {
    width: 350%;
  }
}

@media (max-width: 568px) {
  .mountains {
    width: 200%;
  }
  .clouds {
    width: 650%;
  }

  .rain {
    width: 400%;
  }
}

@media (max-width: 468px) {
  .clouds {
    width: 700%;
  }

  .rain {
    width: 450%;
  }
}

@media (max-width: 425px) {
  .mountains {
    width: 300%;
  }

  .rain {
    width: 500%;
  }
}

@media (max-width: 380px) {
  .clouds {
    width: 750%;
  }

  .rain {
    width: 550%;
  }
}

@media (max-width: 320px) {
  .mountains {
    width: 350%;
  }
  .clouds {
    width: 800%;
  }

  .rain {
    width: 600%;
  }
}

@keyframes cloudsMove {
  to {
    transform: translateX(33.3%);
  }
}

@keyframes rainfall {
  to {
    transform: translate(-5%, 50%);
  }
}
</style>
