<script setup lang="ts">
import { onMounted, onBeforeUnmount, watch, ref, nextTick } from "vue";
const open = defineModel<boolean>("open", { required: true });

const closeButtonRef = ref<HTMLButtonElement | null>(null);
const dialogRef = ref<HTMLElement | null>(null);

const FOCUSABLE =
  'button:not([disabled]), [href], input:not([disabled]), select:not([disabled]), textarea:not([disabled]), [tabindex]:not([tabindex="-1"])';

function getFocusable(): HTMLElement[] {
  if (!dialogRef.value) return [];
  return [...dialogRef.value.querySelectorAll<HTMLElement>(FOCUSABLE)];
}

onMounted(() => {
  window.addEventListener("keydown", handleKeyDown);
});

onBeforeUnmount(() => {
  window.removeEventListener("keydown", handleKeyDown);
});

function handleKeyDown(event: KeyboardEvent) {
  if (event.key === "Escape") {
    if (!open.value) return;
    closeModal();
    return;
  }

  // Focus trap: keep Tab / Shift+Tab inside the dialog while open
  if (!open.value || event.key !== "Tab") return;

  const focusable = getFocusable();
  if (focusable.length === 0) return;

  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  if (event.shiftKey) {
    if (document.activeElement === first) {
      event.preventDefault();
      last.focus();
    }
  } else if (document.activeElement === last) {
    event.preventDefault();
    first.focus();
  }
}

watch(open, (newVal) => {
  if (newVal) {
    nextTick(() => {
      closeButtonRef.value?.focus();
    });
  }
});
function closeModal() {
  open.value = false;
}
</script>

<template>
  <div
    v-if="open"
    class="modal-background"
    @click="closeModal"
    aria-modal="true"
    aria-labelledby="modal-title"
  >
    <div ref="dialogRef" class="modal-container" @click.stop role="dialog">
      <div class="header">
        <h3 id="modal-title">This is the header</h3>
        <button
          class="close"
          @click="closeModal"
          ref="closeButtonRef"
          type="button"
          aria-label="Close modal"
        >
          &times;
        </button>
      </div>
      <div class="content">
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Quisquam, quos.
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Quisquam, quos.
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Quisquam, quos.
      </div>
      <div class="footer">
        <button type="button" class="btn btn-cancel" @click="closeModal">
          Cancel
        </button>
        <button type="button" class="btn btn-save">Save</button>
      </div>
    </div>
  </div>
</template>
<style scoped>
.modal-background {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(1px);
}
.modal-container {
  background: rgb(255, 255, 255);
  min-height: fit-content;
  max-height: 500px;
  min-width: 300px;
  max-width: 500px;
  position: relative;
  margin: 1rem;
  border-radius: 10px;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
}
.modal-container .content {
  min-height: fit-content;
  padding: 0 1rem 1rem 1rem;
  font-size: 1rem;
}
button.close {
  appearance: none;
  background: none;
  border: none;
  padding: 0;
  margin: 0;
  font: inherit;
  color: inherit;
  cursor: pointer;
  position: absolute;
  right: 15px;
  top: 10px;
  font-size: 1.5rem;
}
button.close:focus-visible {
  outline: 3px solid #0066cc;
  outline-offset: 2px;
}
.footer {
  position: relative;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  background: #f0f0f0;
  display: flex;
  justify-content: center;
  gap: 1rem;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}
</style>
