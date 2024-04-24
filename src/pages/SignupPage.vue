<template>
  <div class="q-pa-md">
    <h2 >Sign Up</h2>
    <q-form @submit="signup" class="q-gutter-md q-mx-auto" style="max-width: 50%">
      <q-input rounded standout v-model="username" label="Username" />
      <p style="color: red">{{ usernameError }}</p>

      <q-input rounded standout v-model="email" label="Email" />
      <p style="color : red">{{emailError}}</p>

      <q-input rounded standout v-model="password" label="Password" />
      <p style="color: red;">{{passwordError}}</p>

      <q-input rounded standout v-model="confirmPassword" label="Confirm Password" />
      <p style="color: red;">{{confirmPasswordError}}</p>

      <div class="text-center">
        <q-btn rounded standout class="btn" type="submit" label="SIGN UP" color="primary"/>
      </div>
    </q-form>
  </div>
</template>

<script setup>
import { api } from 'src/boot/axios'
import { useRouter } from 'vue-router'
import { ref } from 'vue'

const username = ref('')
const password = ref('')
const email = ref('')
const confirmPassword = ref('')
const router = useRouter()

const usernameError = ref('')
const emailError = ref('')
const passwordError = ref('')
const confirmPasswordError = ref('')

function signup () {
  api.post('Signup', {
    username: username.value,
    email: email.value,
    password: password.value,
    confirmPassword: confirmPassword.value
  }).then((response) => {
    if (response.status === 200) {
      router.push({ path: '/' })
    } else {
      console.error('Signup Fail', response.data)
    }
  }).catch((error) => {
    console.error('Error Signup : ', error.response.data)
    usernameError.value = error.response.data.messages.message.username
    emailError.value = error.response.data.messages.message.email
    passwordError.value = error.response.data.messages.message.password
    confirmPasswordError.value = error.response.data.messages.message.confirmPassword
  })
}

</script>

<style scoped>
h2 {
  text-align: center;
}
</style>
