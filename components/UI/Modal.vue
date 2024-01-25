<template>
  <div v-if="showModal">
    <Teleport to="#__nuxt">
      <div class="modal-wrapper">
        <div class="modal">
          <div class="modal__header">
            <img class="modal__close" src="/images/close.svg" alt="close" @click="close"/>
          </div>
          <div class="modal__body">
            <slot name="body"></slot>
          </div>
        </div>
        <div class="modal-background" @click="close"/>
      </div>
    </Teleport>
  </div>
</template>

<script setup>
const emit = defineEmits(['closeModal'])
const props = defineProps(['showModal'])

const showModal = computed(() => props.showModal)

function close() {
  emit('closeModal')
}
</script>

<style lang="scss" scoped>
.modal-wrapper {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-background {
  background: rgba(0, 0, 0, 0.25);
  position: absolute;
  height: 100%;
  width: 100%;
  z-index: 99;
}

.modal {
  box-shadow: 0 0 1px rgba(12, 26, 75, 0.24), 0 3px 8px -1px rgba(50, 50, 71, 0.05);
  border-radius: 16px;
  background: #FFFFFF;
  z-index: 100;

  &__header {
    padding: 25px;
    font-size: 22px;
    font-weight: 500;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }

  &__body {
    padding: 10px 15px 25px;
  }

  &__close {
    cursor: pointer;
  }
}
</style>