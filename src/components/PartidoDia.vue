<template>
  <div class="mb-5">
    <TitlePages title="Partidos por Día" />
  </div>
  <input
    type="date"
    id="fecha"
    v-model="fechaSeleccionada"
    @change="cambiarFecha($event.target.value)"
  />
  <table class="table table-info table-striped table-hover">
    <thead>
      <tr>
        <th class="text-center align-middle" scope="col">LOCAL</th>

        <th class="text-center align-middle" scope="col">MARCADOR</th>

        <th class="text-center align-middle" scope="col">VISITANTE</th>
        <th class="text-center align-middle" scope="col">COMPETICION</th>
        <th class="text-center align-middle" scope="col">GRUPO</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="partidoDay in listaPartidosDay" :key="partidoDay.Id">
        <!-- Agregado :key -->
        <td class="text-center align-middle">
          <span
            >{{ partidoDay.Events[0].T1[0].Nm }}
            <img
              :src="`https://lsm-static-prod.livescore.com/medium/${partidoDay.Events[0].T1[0].Img}`"
              class="me-5 ms-3"
              v-bind:alt="partidoDay.Events[0].T1[0].Img"
          /></span>
        </td>
        <td class="text-center align-middle">
          {{ partidoDay.Events[0].Tr1 }} - {{ partidoDay.Events[0].Tr2 }}
        </td>
        <td class="text-center align-middle">
          <span
            ><img
              :src="`https://lsm-static-prod.livescore.com/medium/${partidoDay.Events[0].T2[0].Img}`"
              class="me-5 ms-3"
              :alt="partidoDay.Events[0].T2[0].Img"
            />
            {{ partidoDay.Events[0].T2[0].Nm }}</span
          >
        </td>
        <td class="text-center align-middle">{{ partidoDay.CompN }}</td>
        <td class="text-center align-middle">{{ partidoDay.Snm }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import TitlePages from '../components/TitlePages.vue'
import axios from 'axios'
import { ref, onMounted } from 'vue'

const listaPartidosDay = ref([])
let fechaSeleccionada = ref(new Date())
console.log(fechaSeleccionada.value)

let year = fechaSeleccionada.value.getFullYear()
let month = (fechaSeleccionada.value.getMonth() + 1).toString().padStart(2, '0')
let day = fechaSeleccionada.value.getDate().toString().padStart(2, '0')
let fechaFormateada = year + month + day
console.log(fechaFormateada)

async function getData(date) {
  try {
    console.log(date)
    const { data } = await axios.request({
      method: 'GET',
      url: 'https://livescore6.p.rapidapi.com/matches/v2/list-by-date',
      params: { Category: 'soccer', Date: date, Timezone: '-7' },
      headers: {
        'X-RapidAPI-Key': 'cd269c7843mshf8d04a2f30c4420p1495d4jsn0146dd168b25',
        'X-RapidAPI-Host': 'livescore6.p.rapidapi.com'
      }
    })
    listaPartidosDay.value = data.Stages
    console.log(listaPartidosDay)
  } catch (error) {
    console.error(error)
  }
}

function cambiarFecha(date) {
  if (date) {
    console.log(date)
    fechaFormateada = date.replace(/-/g, '') // reemplaza todos los guiones por una cadena vacía
    console.log(fechaFormateada)
    getData(fechaFormateada)
  }
}

onMounted(() => {
  getData(fechaFormateada)
})
</script>
<style>
table {
  font-size: 1.5vw;
  table-layout: auto;
}
td > img {
  max-width: 25%;
}
span {
  display: inline-flex;
  align-items: center;
}
</style>
