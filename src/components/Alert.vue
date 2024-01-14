<template>
  <div class="blackout" v-show="isShow">
    <div class="alert">
      <span class="alert-message">{{ message }}</span>
      <button class="alert-btn" @click="closeAlert">OK</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isShow: true,
    };
  },
  methods: {
    closeAlert() {
      this.isShow = false;
      this.$emit("closeAlert");
    },
  },
};
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
  z-index: 100;
}

.alert {
  color: black;
  background: #e4deff;
  max-width: 90%;
  border-radius: 0.625rem;
  padding: 1.25rem 1.875rem;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1.875rem;
}

.alert-message {
  font-size: 1.25rem;
}

.alert-btn {
  color: #e4deff;
  background: #1d1d1d;
  width: max-content;
  border: none;
  border-radius: 0.625rem;
  padding: 0.625rem;
  font-size: 1.25rem;
  cursor: pointer;
}

@media (max-width: 425px) {
  .alert {
    padding: 1.25rem;
  }

  .alert-message {
    font-size: 1.125rem;
  }
}
</style>
