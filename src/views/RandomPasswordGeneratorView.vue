<script setup lang="ts">
import Button from '@/components/Button.vue';
import Checkbox from '@/components/Checkbox.vue'
import InputNumbers from '@/components/InputNumbers.vue';
import { ref } from 'vue';
import generate_random_password from '@/composables/usePasswordGenerator.ts'

const passwordLength = ref<number | null>(null)

const options = ref({
  upper: false,
  lower: false,
  number: false,
  symbol: false
})

const generatedPassword = ref('')

const generatePassword = () => {
  if (!passwordLength.value || passwordLength.value <= 0) return

  generatedPassword.value = generate_random_password(
    options.value.upper,
    options.value.lower,
    options.value.number,
    options.value.symbol,
    passwordLength.value
  )
}

</script>

<template>
  <div class="container-random-password-generator">
    <div class="input-numbers">

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
      <Button color="#284520" @click="generatePassword">
        Gerar Senha
      </Button>
    </div>

    <div class="result-password">
      <p> Senha gerada: </p>
      {{ generatedPassword }}
    </div>
  </div>

</template>

<style lang="css" scoped>
.container-random-password-generator {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
}

</style>
