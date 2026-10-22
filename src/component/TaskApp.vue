<script setup>
import { ref, computed } from 'vue'

// Llista de tasques
const tasques = ref([
    {id: 1, titol: 'Tasca 1', completada: false},
    {id: 2, titol: 'Tasca 2', completada: true}
])

const novaTasca = ref('')
const mostrarPendents = ref(false)

// computed recalcula les tasques pendents quan canvia la llista
const tasquesPendents = computed(() => {
  return tasques.value.filter(tasca => !tasca.completada).length // 
})

// Si el filtre està activat, només mostra les tasques pendents
const tasquesVisibles = computed(() => {
  if (mostrarPendents.value) {
    return tasques.value.filter(tasca => !tasca.completada) // Treu les que estiguin completades
  }

  return tasques.value
})

// Afegeix una tasca si l'input no està buit
function afegirTasca() {
  const titol = novaTasca.value.trim()

  if (titol === '') {
    return // Si esta buit no crea res
  }

  tasques.value.push({
    id: Date.now(), // Genera un id a partir de la data actual en milisegons, aixi es unic
    titol: titol,
    completada: false
  })

  novaTasca.value = '' // Despres d'afegirla a la llista s'esborra
}

// Canvia l'estat de la tasca
function canviarEstat(tasca) {
  tasca.completada = !tasca.completada
}

// Elimina una tasca
function eliminarTasca(id) {
  tasques.value = tasques.value.filter(tasca => tasca.id !== id)
}
</script>

<template>
  <main>
    <h1>Gestor de tasques</h1>

    <section>
      <label for="novaTasca">Nova tasca:</label>
      <input v-model="novaTasca" type="text" placeholder="Escriu una nova tasca">
      <button type="button" @click="afegirTasca">Afegir</button>
    </section>

    <section>
      <label>
        <input v-model="mostrarPendents" type="checkbox">Mostrar només tasques pendents</label>
    </section>

    <p v-if="tasquesVisibles.length === 0">No hi ha tasques per mostrar.</p>

    <ul v-else>
      <li v-for="tasca in tasquesVisibles" :key="tasca.id">
        <!-- :class aplica l'estil de completada només si la tasca està completada -->
        <span :class="{ completada: tasca.completada }">{{ tasca.titol }}</span>

        <!--Botó que mostra un missatge o un altre segons si esta completada o no-->
        <button type="button" @click="canviarEstat(tasca)">{{ tasca.completada ? 'Desmarcar' : 'Completar' }}</button>

        <button type="button" @click="eliminarTasca(tasca.id)">Eliminar</button>
      </li>
    </ul>

    <section>
      <p>Total: <b>{{ tasques.length }}</b> | Pendents <b>{{ tasquesPendents }}</b></p>
    </section>
  </main>
</template>

<style>
/* Per distingir les tasques completades */
.completada {
  text-decoration: line-through;
  color: gray;
}
main {
  max-width: 350px;
  margin: 40px auto;
  padding: 20px;
  text-align: center;
  background-color: #f2f2f2;
  border-radius: 10px;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  margin: 10px;
}

button {
  background-color: #6c63ff;
  color: white;
  border: none;
  border-radius: 15px;
  padding: 8px 15px;
  cursor: pointer;
}
</style>
