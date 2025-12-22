<script setup lang="ts">

const UPPER_CASE_LETTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
const LOWER_CASE_LETTERS = "abcdefghijklmnopqrstuvwxyz"
const NUMBERS = "0123456789"
const SYMBOLS = "!\"#$%&'()*+-./:;<=>?@[\]^_`{|}~"

function randomChar(chars: string): string {
  return chars[Math.floor(Math.random() * chars.length)]
}

function shuffle(array: string[]): string[] {
  return array.sort(() => Math.random() - 0.5)
}

function generate_random_password(upper: boolean, lower: boolean, number: boolean, symbols: boolean, password_length: number) {
  let charPool = ""
  let password: string[] = []

  if (upper) charPool += UPPER_CASE_LETTERS
  if (lower) charPool += LOWER_CASE_LETTERS
  if (number) charPool += NUMBERS
  if (symbols) charPool += SYMBOLS

  while (password.length < password_length) {
    password.push(randomChar(charPool))
  }

  return shuffle(password).join("")
}
</script>

<template>

</template>
