<script setup>
import TitlePages from '../components/TitlePages.vue'

import axios from 'axios'
import { ref } from 'vue'

let callApiSoccer = {
  url: 'https://livescore6.p.rapidapi.com/leagues/v2/get-table?Category=soccer&Ccd=spain&Scd=laliga-santander',
  headers: {
    'X-RapidAPI-Key': '2354e7edb9mshf8c9fa220c15434p1804fajsn9793ea2f18a3'
  }
}
const equiposLiga = ref([])

const getData = async () => {
  try {
    const { data } = await axios.request(callApiSoccer)
    console.log(data.LeagueTable.L[0].Tables[0].team)
    equiposLiga.value = data.LeagueTable.L[0].Tables[0].team
  } catch (error) {
    console.log(error)
  }
}
getData()
</script>
<template>
  <div class="mb-5">
    <TitlePages title="Clasificaccion" />
  </div>

  <table class="table table-info table-striped table-hover">
    <thead>
      <tr>
        <th scope="col">Clubes</th>
        <th scope="col">PJ</th>
        <th scope="col">V</th>
        <th scope="col">E</th>
        <th scope="col">D</th>
        <th scope="col">GF</th>
        <th scope="col">GC</th>
        <th scope="col">DG</th>
        <th scope="col">Pts</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="equipo in equiposLiga" key="equipos.Tnm">
        <td scope="row">
          {{ equipo.rnk }}
          <img
            :src="`https://lsm-static-prod.livescore.com/medium/${equipo.Img}`"
            class="me-5 ms-3"
          />
          {{ equipo.Tnm }}
        </td>
        <td>{{ equipo.pld }}</td>
        <td>{{ equipo.win }}</td>
        <td>{{ equipo.drw }}</td>
        <td>{{ equipo.lst }}</td>
        <td>{{ equipo.gf }}</td>
        <td>{{ equipo.ga }}</td>
        <td>{{ equipo.gd }}</td>
        <td>{{ equipo.ptsn }}</td>
      </tr>
    </tbody>
  </table>
</template>
