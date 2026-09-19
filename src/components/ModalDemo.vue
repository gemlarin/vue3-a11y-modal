<script setup lang="ts">
import { ref, watch, nextTick } from "vue";
import A11yModal from "./A11yModal.vue";

const open = ref(false);
const openButtonRef = ref<HTMLButtonElement | null>(null);
watch(open, (newVal) => {
  if (!newVal) {
    nextTick(() => {
      openButtonRef.value?.focus();
    });
  }
});
</script>

<template>
  <div>
    <button type="button" @click="open = true" ref="openButtonRef">
      Open modal
    </button>
    <A11yModal v-model:open="open" />
  </div>
</template>
