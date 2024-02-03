<template>
  <div v-if="isVisible" class="modal">
    <div class="modal-content">
      <slot></slot>
      <button @click="closeModal">Закрыть</button>
    </div>
  </div>
</template>
  
<script>

export default {
  props: {
    isVisible: {
      type: Boolean,
      default: false
    }
  },
  emits: ['close'],
  setup(props, { emit }) {
    const closeModal = () => {
      emit('close');
    };

    return {
      closeModal
    };
  }
};
</script>
  
<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
}
</style>