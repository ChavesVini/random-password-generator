<script setup lang="ts">
import Button from '@/components/Button.vue';
import Checkbox from '@/components/Checkbox.vue'
import InputNumbers from '@/components/InputNumbers.vue';
import { computed, ref } from 'vue';
import {
  generate_random_password,
  calculate_entropy,
  entropy_to_strength
} from '@/composables/usePasswordGenerator'
import { useClipboard } from '@vueuse/core'
import { useToast } from 'vue-toastification';
import Progress from '@/components/Progress.vue';

const toast = useToast()
const passwordLength = ref('')
const generatedPassword = ref('')
const { copy } = useClipboard({
  source: generatedPassword
})
const options = ref({
  upper: false,
  lower: false,
  number: false,
  symbol: false
})
const entropy = computed(() => {
  if (!generatedPassword.value) return 0

  return calculate_entropy(generatedPassword.value)
})

const onCopy = async () => {
  if (!can_generate_password()) return

  await copy()
  toast.success("Password copied!")
}

const can_generate_password = (): boolean => {
  if (!Number(passwordLength.value) || Number(passwordLength.value)  <= 0) {
    toast.error('Select the length of the password!')
    return false
  }

  else if (!options.value.upper && !options.value.lower && !options.value.number && !options.value.symbol) {
    toast.error('Select at least one of the options!')
    return false
  }

  return true
}

const strengthPercentage = computed(() => {
  return Math.min(100, Math.round((entropy.value / 80) * 100))
})

const strengthStatus = computed(() => {
  return entropy_to_strength(entropy.value).status
})

const strengthLabel = computed(() => {
  return entropy_to_strength(entropy.value).label
})

const generate_password = () => {
  if (!can_generate_password()) return

  generatedPassword.value = generate_random_password(
    options.value.upper,
    options.value.lower,
    options.value.number,
    options.value.symbol,
    Number(passwordLength.value)
  )
}
</script>

<template>
  <div class="container-random-password-generator">
    <div class="div-input-numbers">
      <p class="label-select-length-password"> Select the length of the password: </p>
      <InputNumbers
        v-model="passwordLength"
      />

    </div>
    <div class="checkbox-options">
      <Checkbox
        label="Include uppercase letters"
        value="upper"
        size="large"
        v-model="options.upper"
      />

      <Checkbox
        label="Include lowercase letters"
        value="lower"
        size="large"
        v-model="options.lower"
      />

      <Checkbox
        label="Include numbers"
        value="number"
        size="large"
        v-model="options.number"
      />

      <Checkbox
        label="Include symbols"
        value="symbol"
        size="large"
        v-model="options.symbol"
      />
    </div>

    <div class="button-generate-password">
      <Button color="#3b5274" @click="generate_password">
        Generate password
      </Button>
    </div>

    <div class="result-password">
      <p class="label-password-generated"> Password generated: </p>
      <div class="password-box" v-if="generatedPassword">
        {{ generatedPassword }}
      </div>
    </div>

    <div class="progress-strength-password">
      <Progress
        :percentage="strengthPercentage"
        :status_strength="strengthStatus"
      />
      <div class="strength-row">
        <p class="label-strength-password">Password Strength:</p>
        <p class="label-progress-strength-password" :class="strengthStatus">
          {{ strengthLabel }}
        </p>
      </div>
    </div>

    <div class="button-copy-paste">
      <Button color="#3b5274" @click="onCopy">
        Copy password
      </Button>
    </div>
  </div>

</template>

<style lang="css" scoped>
.container-random-password-generator {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
  max-width: 35vw;
  max-height: 50vw;
  margin: auto;
  padding: 20px;
  background-color: #f9fafb;
  border: 1px solid #ddd;
  word-break: break-all;
  border-radius: 8px;
  font-family: Arial, sans-serif;
}

.label-select-length-password, .label-progress-strength-password, .result-password, .label-strength-password{
  color: #000;
}

.label-password-generated {
  text-align: center;
}

:deep(.n-input) {
  --n-height: 44px !important;
  --n-font-size: 16px !important;
  --n-border-radius: 8px !important;
  --n-color: #3b5274 !important;
  --n-text-color: #FFFFFF !important;
  --n-color-focus: #6a6d72 !important;
  --n-border-focus: #6a6d72 !important;
  --n-border-hover: #6a6d72 !important;
  --n-border-disabled: #6a6d72 !important;
  --n-border: #6a6d72 !important;
  --n-caret-color: #6a6d72 !important;
  --n-loading-color: #6a6d72 !important;
  --n-box-shadow-focus: #6a6d72 !important;
}

:deep(.n-checkbox) {
  --n-height: 44px !important;
  --n-font-size: 16px !important;
  --n-color: #3b5274 !important;
  --n-border-checked: #3b5274 !important;
  --n-border-focus: #3b5274 !important;
  --n-border-hover: #3b5274 !important;
  --n-color-checked: #3b5274 !important;
  --n-border: #3b5274 !important;
  --n-box-shadow-focus: #3b5274 !important;
}

.password-box {
  width: 100%;
  max-width: 20vw;
  text-align: center;
  max-height: 20vh;
  background-color: #f1f5f9;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  color: #000;
  overflow-y: auto;
}

.progress-strength-password {
  width: 100%;
  max-width: 300px;
}

.label-progress-strength-password.error {
  color: #dc2626;
}

.label-progress-strength-password.warning {
  color: #f59e0b;
}

.label-progress-strength-password.success {
  color: #16a34a;
}

.strength-row {
  display: flex;
  align-items: center;
  gap: 6px;
}

.label-strength-password,
.label-progress-strength-password {
  margin: 0;
}

@media screen and (max-width: 1024px) {
  .container-random-password-generator {
    max-width: 90vw;
    max-height: 80vh;
  }

  .password-box {
    max-width: 70vw;
  }
}

</style>
