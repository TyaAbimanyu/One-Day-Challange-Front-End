<template>
  <div>
    <q-header>
      <q-toolbar>
        <q-btn rounded standout @click="logout">LOGOUT</q-btn>
      </q-toolbar>
    </q-header>

    <div class="q-pa-md">
    <h2 >Upload Menu</h2>
    <q-form @submit="Upload" class="q-gutter-md q-mx-auto" style="max-width: 50%">
      <q-input rounded standout v-model="menuName" label="Menu Name" />
      <p style="color: red">{{ menunameError }}</p>

      <q-input rounded standout v-model="menuPrice" label="Price" />
      <p style="color : red">{{priceError}}</p>

      <div class="q-gutter-md" style="max-width: 60%">
        <q-file rounded standout bottom-slots v-model="file" label="Upload File" counter>
          <template v-slot:prepend>
            <q-icon name="cloud_upload" @click.stop.prevent/>
          </template>
          <template v-slot:append>
            <q-icon name="close" @click.stop.prevent="model = null" class="cursor-pointer"></q-icon>
          </template>
        </q-file>
      </div>

      <div class="text-center">
        <q-btn rounded standout class="btn" type="submit" label="Upload" color="grey"/>
      </div>
    </q-form>
    </div>

  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
import { ref } from 'vue'
import { api } from 'src/boot/axios'

const router = useRouter()
const file = ref(null)

const menuName = ref('')
const menuPrice = ref('')
const menunameError = ref('')
const priceError = ref('')

function logout () {
  localStorage.removeItem('token')
  router.push({ path: '/' })
}

function Upload () {
  if (!menuName.value && !menuPrice.value && !file.value) {
    console.error('Data is Empty')
  }
  api.post('Upload', {
    menuName: menuName.value,
    menuPrice: menuPrice.value,
    file: file.value
  })
    .then(response => {
      console.log(response.data)
    })
    .catch(error => {
      console.error(error.response.data)
    })
}

</script>

<style scoped>
h2 {
  text-align: center;
}
</style>
