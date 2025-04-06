<script setup lang="ts">
import { ref, computed } from "vue";
import { useStorage } from "@vueuse/core";
import SummaryComponent from "../components/SummaryComponent.vue";
import FilterTypesComponent from "../components/FilterTypesComponent.vue";
import ListMoveComponent from "../components/ListMoveComponent.vue";

const typeSelected = ref("");
const allMoves = useStorage("movimientos", [] as any[]);

function movimiento(n: string) {
  typeSelected.value = n;
}

const filteredMoves = computed(() => {
  if (!typeSelected.value || typeSelected.value === "All")
    return allMoves.value;
  return allMoves.value.filter((m) => m.type === typeSelected.value);
});

const inputs = computed(() =>
  allMoves.value
    .filter((m) => m.type === "Ingresos")
    .reduce((a, b) => a + b.amount, 0)
);
const outputs = computed(() =>
  allMoves.value
    .filter((m) => m.type === "Salidas")
    .reduce((a, b) => a + b.amount, 0)
);
const balance = computed(() => inputs.value - outputs.value);
</script>

<template>
  <SummaryComponent :balance="balance" :inputs="inputs" :outputs="outputs" />
  <FilterTypesComponent @movimiento="movimiento" />
  <ListMoveComponent :moves="filteredMoves" />
</template>
