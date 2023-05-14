<script setup>
import { ref, onMounted } from 'vue'

const currentDate = new Date().getFullYear()
const message = ref()
const key = ref()
const result = ref()

function vigenereEncrypt() {
  var ciphertext = ''
  var j = 0

  for (let i = 0; i < message.value.toUpperCase().length; i++) {
    let charCode = message.value.toUpperCase().charCodeAt(i)
    if (charCode >= 65 && charCode <= 90) {
      let keyChar = key.value.charCodeAt(j % key.value.length) - 65
      let encryptedCharCode = ((charCode - 65 + keyChar) % 26) + 65
      ciphertext += String.fromCharCode(encryptedCharCode)
      j++
      // } else if (charCode >= 97 && charCode <= 122) {
      //   let keyChar = key.value.charCodeAt(j % key.value.length) - 97
      //   let encryptedCharCode = ((charCode - 97 + keyChar) % 26) + 97
      //   ciphertext += String.fromCharCode(encryptedCharCode)
      //   j++
    } else if (charCode <= 64) {
      result.value += ciphertext += message.value.toUpperCase().charAt(i)
    }
  }

  return (result.value = ciphertext)
}

function vigenereDecrypt() {
  var plaintext = ''
  var j = 0

  for (let i = 0; i < message.value.toUpperCase().length; i++) {
    let charCode = message.value.toUpperCase().charCodeAt(i)
    if (charCode >= 65 && charCode <= 90) {
      let keyChar = key.value.charCodeAt(j % key.value.length) - 65
      let decryptedCharCode = ((charCode - 65 - keyChar + 26) % 26) + 65
      plaintext += String.fromCharCode(decryptedCharCode)
      j++
      // } else if (charCode >= 97 && charCode <= 122) {
      //   let keyChar = key.value.charCodeAt(j % key.value.length) - 97
      //   let decryptedCharCode = ((charCode - 97 - keyChar + 26) % 26) + 97
      //   plaintext += String.fromCharCode(decryptedCharCode)
      //   j++
    } else {
      result.value += plaintext += message.value.toUpperCase().charAt(i)
    }
  }

  return (result.value = plaintext)
}

onMounted(() => {
  currentDate, message, key, vigenereEncrypt, vigenereDecrypt
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
                  placeholder="Masukan Plain Teks Disini"
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
                  class="block placeholder:text-gray-400 w-full rounded-md border-0 py-1.5 px-1.5 text-gray-600 shadow-sm ring-1 ring-inset ring-gray-600"
                />
              </div>
            </div>

            <div class="col-span-6">
              <button
                @click.prevent="vigenereEncrypt"
                class="p-3 mr-3 bg-green-400 text-gray-900 hover:bg-green-500 rounded-md"
              >
                Enkripsi
              </button>
              <button
                @click.prevent="vigenereDecrypt"
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
