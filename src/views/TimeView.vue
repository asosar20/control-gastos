<template>
    <div class="flex items-center justify-between px-10 pt-10">
        <h1 class="text-3xl font-bold text-primary">Movimientos</h1>
        <RouterLink to="/">
            <button class="btn btn-sm btn-dash btn-primary">Volver</button>
        </RouterLink>
    </div>
    <ul class="timeline timeline-vertical lg:timeline-horizontal pt-5" >
        <li v-for="move in ordenados" :key="move.id">
            <hr :class="{
                'bg-accent': move.type === 'Ingresos',
                'bg-secondary': move.type === 'Salidas',
            }" />
            <div class="timeline-box"
                :class="{
                    'timeline-start ': move.type === 'Ingresos',
                    'timeline-end ': move.type === 'Salidas',
                }"
            >
                <p :class="{
                        'text-accent': move.type === 'Ingresos',
                        'text-secondary': move.type === 'Salidas',
                    }">{{move.date}}</p>
                <div class="badge badge-dash mt-2"
                    :class="{
                        'text-accent badge-accent': move.type === 'Ingresos',
                        'text-secondary badge-secondary': move.type === 'Salidas',
                    }"
                >
                    ${{move.amount}}
                </div>
            </div>
            <div class="timeline-middle">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" 
                    :class="{
                        'stroke-accent': move.type === 'Ingresos',
                        'stroke-secondary': move.type === 'Salidas',
                    }"
                    class="size-6">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 18.75a60.07 60.07 0 0 1 15.797 2.101c.727.198 1.453-.342 1.453-1.096V18.75M3.75 4.5v.75A.75.75 0 0 1 3 6h-.75m0 0v-.375c0-.621.504-1.125 1.125-1.125H20.25M2.25 6v9m18-10.5v.75c0 .414.336.75.75.75h.75m-1.5-1.5h.375c.621 0 1.125.504 1.125 1.125v9.75c0 .621-.504 1.125-1.125 1.125h-.375m1.5-1.5H21a.75.75 0 0 0-.75.75v.75m0 0H3.75m0 0h-.375a1.125 1.125 0 0 1-1.125-1.125V15m1.5 1.5v-.75A.75.75 0 0 0 3 15h-.75M15 10.5a3 3 0 1 1-6 0 3 3 0 0 1 6 0Zm3 0h.008v.008H18V10.5Zm-12 0h.008v.008H6V10.5Z" />
                </svg>
            </div>
            <div class="timeline-box"
            :class="{
                    'timeline-end ': move.type === 'Ingresos',
                    'timeline-start ': move.type === 'Salidas',
                }"
            >
                <p :class="{
                        'text-accent': move.type === 'Ingresos',
                        'text-secondary': move.type === 'Salidas',
                    }">{{move.name}}</p>
                <div class="badge badge-dash text-xs mt-2"
                    :class="{
                        'text-accent badge-accent': move.type === 'Ingresos',
                        'text-secondary badge-secondary': move.type === 'Salidas',
                    }"
                >
                    {{move.type}}
                </div>
            </div>
            <hr :class="{
                'bg-accent': move.type === 'Ingresos',
                'bg-secondary': move.type === 'Salidas',
            }" />
        </li>
    </ul>
</template>

<script setup lang="ts">
import { useStorage } from '@vueuse/core';
import { computed } from 'vue';

const allMoves = useStorage("movimientos", [] as any[]);

const ordenados = computed(() => 
  [...allMoves.value].sort((a, b) => 
    new Date(b.isoDate).getTime() - new Date(a.isoDate).getTime()
  )
);

</script>