<script setup lang="ts">

import {NProgress} from 'naive-ui'

defineProps<{
  percentage: number
  status_strength: 'default' | 'success' | 'error' | 'warning' | 'info'
}>()

const emit = defineEmits<(e: 'update:modelValue', value: string | [string, string] | null | undefined) => void>()

</script>

<template>
    <div class="div-check-password-strength">
        <n-progress
            type="line"
            :show-indicator="false"
            :percentage=percentage
            :status="status_strength"
        />
    </div>
</template>