<template>
  <transition name="modal-animation">
    <div v-show="props.modalOpen" class="modal">
      <div class="innerModal">
        <div class="closeIcon">
          <i @click="$emit('closeModal')" class="fa-solid fa-circle-xmark"></i>
        </div>
        <slot></slot>
        <button type="button" class="closeBtn" @click="$emit('closeModal')">
          Close
        </button>
      </div>
    </div>
  </transition>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps(["modalOpen"]);
</script>

<style lang="scss" scoped>
.modal-animation-enter-active,
.modal-animation-leave-active {
  transition: opacity 1s ease;
}

.modal-animation-enter-from,
.modal-animation-leave-to {
  opacity: 0;
}
.modal {
  background-color: rgb(49, 34, 16);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  .innerModal {
    width: 550px;
    height: 250px;
    border-radius: 20px;
    background-color: rgb(241, 237, 237);
    color: #333;
    padding: 20px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: start;
    flex-direction: column;
    gap: 20px;
    box-shadow: 4px 6px 2px 1px rgba(0, 0, 0, 0.3),
      6px 4px 2px 1px rgba(0, 0, 0, 0.3);

    .closeIcon {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 20px;
      cursor: pointer;

      i {
        transition: all 0.4s;
        &:hover {
          color: rgb(161, 51, 51);
        }
      }
    }

    .closeBtn {
      color: #fff;
      font-size: 16px;
      padding: 7px 12px;
      border-radius: 10px;
      border: 2px solid #333;
      background-color: rgba(196, 63, 29, 0.986);
      cursor: pointer;

      &:hover {
        opacity: 0.9;
      }
    }
  }
}
</style>