<script setup lang="ts">
const emit = defineEmits(["movimiento"]);
function seleccionarTipo(tipo: string) {
  emit("movimiento", tipo);
}
</script>

<template>
  <div class="flex justify-center pt-5">
    <div class="filter">
      <button class="btn" @click="seleccionarTipo('')">Todos</button>
      <button class="btn" @click="seleccionarTipo('Ingresos')">Ingresos</button>
      <button class="btn" @click="seleccionarTipo('Salidas')">Salidas</button>
    </div>
  </div>
</template>
