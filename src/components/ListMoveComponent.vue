<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  moves: {
    id: string;
    name: string;
    type: string;
    date: string;
    amount: number;
  }[];
}>();

const sortedMoves = computed(() => {
  return [...props.moves].sort((a, b) => b.id.localeCompare(a.id));
});

</script>

<template>
  <div class="px-5">
    <ul class="list bg-base-100 rounded-box shadow-md">
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
    </ul>
  </div>
</template>
