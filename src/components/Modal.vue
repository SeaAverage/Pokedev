<script setup>
const props = defineProps({
  show: Boolean
})
</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container">
        <div class="modal-close">
          <slot name="close">
            <button
              class="modal-default-button"
              @click="$emit('close')"
            >&#10006;</button>
          </slot>
        </div>
        <div class="modal-header">
          <slot name="header">default header</slot>
        </div>

        <div class="modal-body">
          <slot name="body">default body</slot>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: opacity 0.3s ease;
}

.modal-container {
  width: 300px;
  min-height: 200px;
  margin: auto;
  padding: 24px 24px;
  padding-bottom: 12px;
  background: linear-gradient(180deg, red 30%, white 30%);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: white;
}

.modal-body {
  margin: 20px 0;
  color: black;
  padding-top: 8px;
  padding-bottom: 8px;
}

.modal-default-button {
  cursor: pointer;
  font-size: 16px;
  color: white;
  margin-top: -16px;
  margin-right: -16px;
  border: none;
  background-color: inherit;
  float: right;
}

.modal-default-button:hover{
  background:  rgba(0, 0, 0, 0.2);
}

.modal-default-button:focus{
  color:  rgb(194, 194, 194);
}


/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>