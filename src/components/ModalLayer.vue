<template>
    <div class="modal">
      <div class="modal__overlay"></div>
      <div class="modal__content">
        <div class="modal__header">
          <slot name="header"></slot>
          <button class="modal__close" @click="closeModal">
            <img src="@/assets/images/close-button.svg" alt="Cerrar">
          </button>
        </div>
        <div class="modal__body">
          
          <slot name="body"></slot>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ModalLayer',
    methods: {
      closeModal() {
        //Event when closing the modal, to use in the future.
        this.$emit('close');
      },
    },
  };
  </script>
  
  <style scoped>
  .modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 999;
  }
  .modal__overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
  }
  .modal__content {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 90%;
    max-width: 500px;
    height: 90%;
    max-height: 600px;
    transform: translate(-50%, -50%);
    background-color: #fff;
    border-radius: 4px;
    padding: 30px;
    overflow: auto;
  }
  .modal__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    position: relative;
  }
  .modal__close {
    background-color: transparent;
    border: none;
    cursor: pointer;
    position: absolute;
    top: 0;
    right: 0;
  }
  .modal__close img {
    width: 20px;
  }
  </style>
  