<script setup>
import { ref, computed } from 'vue'

// Llista de tasques
const tasques = ref([
    {id: 1, titol: 'Tasca 1', completada: false}
])

</script>

<template>
    <h1>Gestor de tasques</h1>
</template>