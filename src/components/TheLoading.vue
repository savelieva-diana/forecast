<template>
  <Transition name="fade">
    <div class="loading" v-if="loading">
      <div class="loading__spinner"></div>
    </div>
  </Transition>
</template>

<script setup>
defineProps({
  loading: {
    type: Boolean,
    required: true
  }
});
</script>

<style lang="scss" scoped>
.loading {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  z-index: 10000;
  background-color: #59585d;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  &__spinner {
    display: inline-block;
    width: 64px;
    height: 64px;
    border: 4px solid white;
    border-radius: 50%;
    border-top-color: #59585d;
    animation: spin 1s ease-in-out infinite;
  }
}
@keyframes spin {
  to {
    -webkit-transform: rotate(360deg);
  }
}

.fade-leave-active {
  transition: all 0.3s ease;
}

.fade-leave-to {
  opacity: 0;
}
</style>
