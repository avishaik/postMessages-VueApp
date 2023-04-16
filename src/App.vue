<script setup lang="ts">
import { computed, ref } from "vue";
import Navbar from './components/Navbar.vue';
import FormInput from "./components/FormInput.vue";
import { useModal } from "./composables/modal";
import { useUsers } from "./stores/users";
import { validate, length, required } from "./validation";

const modal = useModal();
const userStore = useUsers();

const modalStyle = computed(() => {
  return {
    display: modal.show.value ? 'block' : 'none'
  }
});

const username = ref('');
const usernameStatus = computed(() => {
  return validate(username.value, [required, length({ min: 5, max: 20})]);
})


</script>

<template>
  <div class="modal" style="color: white;" :style="modalStyle">
    <div class="div modal-background">
      <div class="div modal-content">
        <div id="modal"></div>
      </div>
    </div>
    <button class="modal-close is-large" @click="modal.hideModal()"></button>
  </div>
  <div class="section">
    <div class="container">
      <Navbar />
      <RouterView />
      {{ username }}
    </div>
  </div>
</template>

<style scoped>
@import "https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css";
@import "highlight.js/styles/atom-one-dark.css";

ul {
  list-style: revert !important;
  list-style-position: inside !important;
}
h1, h2, h3, h4, h5, h6 {
  font-size: revert !important;
  margin: 10px 0 !important;
}
pre {
  margin: 10px 0 !important;
}
p {
  margin: 10px 0 !important;
}
div[contenteditable] {
  white-space: pre;
}
</style>
