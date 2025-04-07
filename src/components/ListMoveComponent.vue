<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  moves: {
    id: string;
    name: string;
    type: string;
    date: string;
    amount: number;
    isoDate: string;
  }[];
}>();

const sortedMoves = computed(() => {
  return [...props.moves].sort((a, b) => new Date(b.isoDate).getTime() - new Date(a.isoDate).getTime());
});


</script>

<template>
  <div class="px-5">
    <ul class="list bg-base-100 rounded-box shadow-md">
      <template v-if="moves.length > 0">
        <li class="p-4 pb-2 text-xs opacity-60 tracking-wide">
        Registros de este mes
      </li>

      <li class="list-row" v-for="move in sortedMoves" :key="move.id">
        <div class="list-col-grow">
          <div>{{ move.name }}</div>
          <div
            class="badge badge-dash text-xs mt-2"
            :class="{
              'text-accent badge-accent': move.type === 'Ingresos',
              'text-secondary badge-secondary': move.type === 'Salidas',
            }"
          >
            {{ move.type }}
          </div>
        </div>
        <p class="list-col-wrap text-xs">{{ move.date }}</p>
        <div class="flex items-center gap-2">
          <div
            class="badge badge-dash text-right text-xl p-4"
            :class="{
              'text-accent badge-accent': move.type === 'Ingresos',
              'text-secondary badge-secondary': move.type === 'Salidas',
            }"
          >
            ${{ move.amount }}
          </div>
        </div>
      </li>
      </template>
      <template v-else>
        <li class="p-8 text-center">
          <div class="opacity-50 mb-4">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-12 h-12 mx-auto mb-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
            </svg>
            <p class="text-lg font-semibold">No hay registros</p>
          </div>
          <RouterLink to="/new-input" class="btn btn-accent btn-sm">
            Agregar primer movimiento
          </RouterLink>
        </li>
      </template>
    </ul>
  </div>
</template>
