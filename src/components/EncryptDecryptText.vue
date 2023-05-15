<script setup>
import { ref, onMounted } from 'vue'

const currentDate = new Date().getFullYear()
const message = ref()
const key = ref()
const result = ref()
const encrypted = ref('')
const decrypted = ref('')

function isUpperCase(letter) {
  const l = letter.charCodeAt()
  if (l > 64 && l < 91) {
    return true
  } else {
    return false
  }
}

function isLowerCase(letter) {
  const l = letter.charCodeAt()
  if (l > 96 && l < 123) {
    return true
  } else {
    return false
  }
}

function mod(n, m) {
  return ((n % m) + m) % m
}

function encrypt(message, key) {
  let encrypted = ''
  let j = 0
  for (let i = 0; i < message.length; i++) {
    let currentLetter = message[i]
    const A = 65
    const a = 97

    if (isUpperCase(currentLetter)) {
      let Pi = currentLetter.charCodeAt(0) - A
      let Ki = key[j % key.length].toUpperCase().charCodeAt() - A
      let upperLetter = mod(Pi + Ki, 26)

      encrypted += String.fromCharCode(upperLetter + A)

      j++
    } else if (isLowerCase(currentLetter)) {
      let Pi = currentLetter.charCodeAt() - a
      let Ki = key[j % key.length].toLowerCase().charCodeAt() - a
      let lowerLetter = mod(Pi + Ki, 26)

      encrypted += String.fromCharCode(lowerLetter + a)

      j++
    } else {
      encrypted += currentLetter
    }
  }
  return (result.value = encrypted)
}

function decrypt(enc, key) {
  let decrypted = ''
  let j = 0
  for (let i = 0; i < enc.length; i++) {
    let currentLetter = enc[i]
    const A = 65
    const a = 97

    if (isUpperCase(currentLetter)) {
      let Ci = currentLetter.charCodeAt(0) - A
      let Ki = key[j % key.length].toUpperCase().charCodeAt() - A
      let upperLetter = mod(Ci - Ki, 26)

      decrypted += String.fromCharCode(upperLetter + A)

      j++
    } else if (isLowerCase(currentLetter)) {
      let Ci = currentLetter.charCodeAt() - a
      let Ki = key[j % key.length].toLowerCase().charCodeAt() - a
      let lowerLetter = mod(Ci - Ki, 26)

      decrypted += String.fromCharCode(lowerLetter + a)

      j++
    } else {
      decrypted += currentLetter
    }
  }
  return (result.value = decrypted)
}

function handleEncryptClick() {
  encrypted.value = encrypt(message.value, key.value)
}

function handleDecryptClick() {
  decrypted.value = decrypt(encrypted.value, key.value)
}

onMounted(() => {
  currentDate, message, key, encrypt, decrypt, encrypted, decrypted
})
</script>

<template>
  <div class="container mx-auto">
    <div class="w-full max-w my-10">
      <form
        class="bg-gradient-to-r from-green-300 to-green-400 backdrop-blur shadow-md rounded-md px-8 pt-6 pb-8 mb-4"
      >
        <div>
          <h1 class="text-2xl text-center font-semibold leading-7 text-gray-950">
            Enkripsi dan Dekripsi Teks
          </h1>

          <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
            <div class="col-span-6">
              <label for="message" class="block text-base font-medium leading-6 text-gray-950"
                >Pesan</label
              >
              <div class="mt-2">
                <input
                  pattern="[A-Z\s]+"
                  type="text"
                  name="message"
                  id="message"
                  placeholder="Masukan Plain Teks atau Cipher Teks Disini"
                  v-model="message"
                  class="block bg-gray-200 w-full rounded-md border-0 py-1.5 px-1.5 text-gray-950 ring-1 ring-gray-600 placeholder:text-gray-400 focus:outline-none focus:ring focus:ring-gray-800"
                />
              </div>
              <p class="text-gray-800">*Ditulis dengan huruf kapital</p>
            </div>

            <div class="col-span-6">
              <label for="key" class="block text-base font-medium leading-6 text-gray-950"
                >Key</label
              >
              <div class="mt-2">
                <input
                  pattern="[A-Z\s]+"
                  type="text"
                  name="key"
                  id="key"
                  placeholder="Masukan Key Disini"
                  v-model="key"
                  class="block bg-gray-200 w-full rounded-md border-0 py-1.5 px-1.5 text-gray-950 ring-1 ring-gray-600 placeholder:text-gray-400 focus:outline-none focus:ring focus:ring-gray-800"
                />
              </div>
              <p class="text-gray-800">*Ditulis dengan huruf kapital dan tanpa spasi</p>
            </div>

            <div class="col-span-6">
              <label for="result" class="block text-base font-medium leading-6 text-gray-950"
                >Hasil</label
              >
              <div class="mt-2">
                <input
                  disabled
                  type="text"
                  name="result"
                  id="result"
                  :value="result"
                  placeholder="Hasil Enkripsi dan Dekripsi"
                  class="block placeholder:text-gray-500 w-full rounded-md border-0 py-1.5 px-1.5 text-gray-600 shadow-sm ring-1 ring-inset ring-gray-600"
                />
              </div>
            </div>

            <div class="col-span-6">
              <button
                @click.prevent="handleEncryptClick"
                class="p-3 mr-3 bg-green-400 text-gray-900 hover:bg-green-500 rounded-md"
              >
                Enkripsi
              </button>
              <button
                @click.prevent="handleDecryptClick"
                class="p-3 bg-green-400 text-gray-900 hover:bg-green-500 rounded-md"
              >
                Dekripsi
              </button>
            </div>
          </div>
        </div>
      </form>
      <p class="text-center text-gray-500 text-xs">
        Copyright &copy; {{ currentDate }} All rights reserved - mluthfifarid
      </p>
    </div>
  </div>
</template>
