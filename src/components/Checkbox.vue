<script setup lang="ts">
import { NSpace, NCheckbox } from 'naive-ui'

defineProps<{
  modelValue: boolean
  label: string
  size?: 'small' | 'medium' | 'large'
}>()

const emit = defineEmits<(e: 'update:modelValue', value: boolean) => void>()
</script>

<template>
  <div class="div-generate-password">
    <n-space item-style="display: flex;" align="center">
      <n-checkbox
        :checked="modelValue"
        :label="label"
        :size="size ?? 'large'"
        @update:checked="emit('update:modelValue', $event)"
      />
    </n-space>
  </div>
</template>
