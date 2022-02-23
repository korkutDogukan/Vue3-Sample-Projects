<template>
  <div class="home" :class="backgroundColor">
    <Modal :modalOpen="modalOpen" @closeModal="toggleModal">
      <h2>This is a Modal Header</h2>
      <p>This is a modal message</p>
    </Modal>
    <button
      v-show="modalOpen===false"
      type="button"
      class="openModal"
      @click="toggleModal"
    >
      Open Modal
    </button>
  </div>
</template>

<script setup>
import Modal from "@/components/Modal.vue";
import { ref, computed } from "vue";

const modalOpen = ref(false);

const toggleModal = () => {
  modalOpen.value = !modalOpen.value;
};

const backgroundColor = computed(() => {
  if (modalOpen.value === true) {
    return "openModalBG";
  } else {
    return false;
  }
});
</script>

<style lang="scss" scoped>
.openModalBG {
  background-color: rgb(49, 34, 16) !important;
}
.home {
  width: 100%;
  height: 100vh;
  position: relative;
  background-color: rgb(156, 124, 84);
  display: flex;
  justify-content: center;
  align-items: center;

  h2 {
    font-size: 35px;
    text-align: center;
  }

  p {
    font-size: 20px;
    font-weight: 600;
  }

  .openModal {
    padding: 15px 30px;
    font-size: 20px;
    font-weight: 700;
    color: #fff;
    border: 2px solid rgb(250, 250, 250);
    border-radius: 15px;
    background-color: rgb(196, 131, 57);
    box-shadow: 4px 6px 2px 1px rgba(0, 0, 0, 0.3),
      6px 4px 2px 1px rgba(0, 0, 0, 0.3);
    cursor: pointer;

    &:hover {
      opacity: 0.85;
    }
  }
}
</style>
