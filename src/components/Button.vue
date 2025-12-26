<script setup lang="ts">

import {NButton} from 'naive-ui'

defineProps<{
  color: string
}>()

</script>

<template>
    <div class="div-generate-password">
        <n-button :color="color">
            <slot />
        </n-button>
    </div>
</template>