<template>
  <div class="q-pa-md">
    <h2>Login</h2>
    <q-form @submit="login" class="q-gutter-md q-mx-auto" style="max-width: 50%">
      <q-input rounded standout v-model="username" label="Username"/>
      <p style="color: red;">{{usernameError}}</p>
      <q-input rounded standout v-model="password" label="Password" />
      <p style="color: red;">{{passwordError}}</p>
      <div class="text-center">
        <q-btn rounded standout type="submit" label="LOGIN" color="primary"/>
      </div>
    </q-form>
    <p></p>
    <div class="registerLink">
      <p></p>
      Not a Member? <router-link to="Signup">Signup</router-link>
    </div>
  </div>
</template>

<script setup>
import { api } from 'src/boot/axios'
import { useRouter } from 'vue-router'
import { ref } from 'vue'

const username = ref('')
const password = ref('')
const router = useRouter()
const usernameError = ref('')
const passwordError = ref('')

function login () {
  api.post('Login', {
    username: username.value,
    password: password.value
  }).then((response) => {
    if (response.status === 200) {
      const token = response.data.token
      if (token) {
        localStorage.setItem('token', token)
        router.push({ path: 'Home' })
        console.log('Token : ', token)
      } else {
        console.error('Token Not Found', response)
      }
    } else {
      console.error('Login Fail', response.data)
    }
  }).catch((error) => {
    console.error('Error Signup : ', error.response.data)
    usernameError.value = error.response.data.messages.message.username
    passwordError.value = error.response.data.messages.message.password
  })
}

</script>

<style scoped>
h2 {
  text-align: center;
}
.registerLink {
  text-align: center;
  margin-top: 20px;
  font-size: 0.8em;
}
</style>
