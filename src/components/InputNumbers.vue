<script setup lang="ts">

import {NInput} from 'naive-ui'

defineProps<{
  modelValue: string | [string, string] | null | undefined
}>()

const emit = defineEmits<(e: 'update:modelValue', value: string | [string, string] | null | undefined) => void>()

const onlyAllowNumber = (value: string) => !value || /^\d+$/.test(value)

</script>

<template>
    <div class="div-generate-password">
        <n-input
            :value="modelValue"
            :show-button="false"
            @update:value="emit('update:modelValue', $event)"
            :allow-input="onlyAllowNumber"
        />
    </div>
</template>