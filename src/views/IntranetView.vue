<script setup>
import TitlePages from '../components/TitlePages.vue'
import{ ref } from 'vue'
import axios from 'axios'

const titulo = ref([])
const author = ref([])
const contenido = ref([])

var config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'http://localhost:3000/posts',
  headers: {
    'Content-Type': 'application/json'
  },
  
}

const createPost = async () => {

  const response = await axios(config,{
    
    body: JSON.stringify({
    titulo: titulo.value,
    author: author.value,
    contenido: contenido.value,
  })
  })
  
}


</script>

<template>
  <TitlePages title="IntraNet" />

  <div class="intranet">
    <h2 class="my-5">postea en el blog</h2>

    <form class="form" action="">
      <div class="inputs">
        <label for="titulo">Titulo: </label>
        <input class="rounded" type="text" name="titulo" id="titulo" v-model="titulo" />
      </div>

      <div class="inputs">
        <label for="author">Author: </label>
        <input class="rounded" type="text" name="author" id="author" v-model="author" />
      </div>
      <div class="inputs-text">
        <textarea
          class="rounded"
          name="contenido"
          id="contenido"
          cols="90"
          rows="10"
          v-model="contenido"
        ></textarea>
      </div>
      <div class="inputs-boton">
        <input class="rounded btn btn-info" type="submit" name="enviar" id="enviar" />
      </div>
    </form>
  </div>
</template>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.inputs {
  margin: 1rem 0;
}

.inputs-text {
  margin: 1rem 0;
}

.inputs-boton {
  margin: 1rem 0;
}
</style>
