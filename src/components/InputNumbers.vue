<script setup lang="ts">

import {NInputNumber} from 'naive-ui'

defineProps<{
  modelValue: number | null
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: number | null): void
}>()

</script>

<template>
    <div class="div-generate-password">
        <n-input-number
            :value="modelValue"
            :show-button="false"
            @update:value="emit('update:modelValue', $event)"
        />
    </div>
</template>

<style lang="css" scoped> 
.button-generate-password {
    color: #fff
}

:deep(.n-input) {
  --n-height: 44px !important;
  --n-font-size: 16px !important;
  --n-border-radius: 8px !important;
  --n-text-color: #ffffff !important;
  --n-color: #800080 !important;
  --n-color-focus: rgb(62, 0, 62) !important;
  --n-border-focus: rgb(62, 0, 62) !important;
  --n-border-hover: rgb(62, 0, 62) !important;
  --n-border-disabled: rgb(62, 0, 62) !important;
  --n-border: rgb(62, 0, 62) !important;
  --n-caret-color: rgb(62, 0, 62) !important;
  --n-loading-color: rgb(62, 0, 62) !important;
  --n-box-shadow-focus: rgb(62, 0, 62) !important;
}

</style>