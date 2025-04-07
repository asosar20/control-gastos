<script setup lang="ts">
const props = defineProps<{
  balance: number;
  inputs: number;
  outputs: number;
  primeraFecha: string;
  ultimaFecha: string;
}>();
</script>

<template>
  <div class="flex justify-center pt-10">
    <div class="stats stats-vertical md:stats-horizontal shadow">
      <div class="stat place-items-center">
        <div class="stat-figure text-primary">
        </div>
        <div class="stat-title">Saldo</div>
        <div class="stat-value text-primary">
          ${{ props.balance.toLocaleString() }}
        </div>
        <div class="stat-actions">
          <RouterLink to="/new-input?tipo=Ingresos">
            <button class="btn btn-xs btn-dash btn-primary mr-2">
            Nuevo Ingreso
          </button>  
          </RouterLink>

          <RouterLink to="/new-input?tipo=Salidas">
            <button class="btn btn-xs btn-dash btn-primary mr-2">
            Nuevo Salida
          </button>  
          </RouterLink>
        </div>
        <div class="stat-desc mt-1 text-primary" v-if="props.primeraFecha">
          Del {{props.primeraFecha}} al {{props.ultimaFecha}}
        </div>
        <div v-elfe class="stat-desc mt-1 text-primary">
          No hay registros
        </div>
      </div>

      <div class="stat place-items-center">
        <div class="stat-figure text-accent">
        </div>
        <div class="stat-title">Ingresos</div>
        <div class="stat-value text-accent">
          ${{ props.inputs.toLocaleString() }}
        </div>
        <div class="stat-actions">
          <button class="btn btn-xs btn-dash btn-accent">Ver todo</button>
        </div>
        <div class="stat-desc text-accent mt-1">↗︎</div>
      </div>

      <div class="stat place-items-center">
        <div class="stat-figure text-secondary">
        </div>
        <div class="stat-title">Salidas</div>
        <div class="stat-value text-secondary">
          ${{ props.outputs.toLocaleString() }}
        </div>
        <div class="stat-actions">
          <button class="btn btn-xs btn-dash btn-secondary">Ver todo</button>
        </div>
        <div class="stat-desc text-secondary mt-1">↘︎</div>
      </div>
    </div>
  </div>
</template>
