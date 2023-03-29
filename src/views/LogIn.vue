<script setup>
import axios from 'axios'
import { ref } from 'vue'




const useradmin = ref("")
const password = ref("")



var config = {
  method: 'get',
  url: 'http://localhost:3000/usuarios',
};


const getUserAdmin = async () => {
  try {
    const data = await axios.request(config)
    useradmin.value = data.data[0].useradmin
    password.value= data.data[0].password
  } catch (error) {
    console.log(" a habido un error ", error)
  }
}
getUserAdmin();

const formtData = ref({
  username: '',
  password: ''
})


function getDataAdmin(data) {
    if (useradmin.value == data.username && password.value == data.password){
     window.location.href = "/intranet"
    } else if (useradmin.value!= data.username || password.value!= data.password){
      alert("Usuario o contraseña incorrectos")
    }
    
}

</script>
<template>
  <div class="formularioRegistro">
    <div class="formularios">
      <FormKit type="form"
       submit-label="Login"
        @submit="getDataAdmin"
        :value="formtData"
        >

        <h2 class="mb-5">Login Admin</h2>

      <FormKit type="text"
       label="User Name"
        name="username"
        
        />

      <FormKit
      type="password"
      name="password"
      label="Password"
      
    />

      </FormKit>
    </div>
  </div>
</template>

<style scoped>
.formularioRegistro {
  padding: 5rem 0;
  background-color: #acd6d4;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.formularios {
  background-color: rgb(252, 250, 247);
  border: 1px solid black;
  border-radius: 10px;
  padding: 6rem;
}
</style>
