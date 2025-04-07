<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useStorage } from "@vueuse/core";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();
const emit = defineEmits(["guardar"]);

const nombre = ref("");
const monto = ref<number | null>(null);
const tipo = ref(route.query.tipo?.toString() || "Ingresos");
const fecha = ref("");
const hora = ref("");

const movimientos = useStorage("movimientos", [] as any[]);

onMounted(() => {
  const options: Intl.DateTimeFormatOptions = { 
    timeZone: 'America/Lima',
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    hour: '2-digit',
    minute: '2-digit',
    hour12: false
  };
  
  const peruDateTime = new Date().toLocaleString('en-US', options);
  const [date, time] = peruDateTime.split(', ');
  const [month, day, year] = date.split('/');
  
  fecha.value = `${year}-${month}-${day}`;
  hora.value = time;
});

function guardar(salir = false) {
  if (!nombre.value || !monto.value || !fecha.value || !hora.value || monto.value <= 0) return;

  const [year, month, day] = fecha.value.split('-');
  const [hours, minutes] = hora.value.split(':');
  
  const fechaCompleta = new Date(
    Date.UTC(
      parseInt(year),
      parseInt(month) - 1,
      parseInt(day),
      parseInt(hours) + 5, // Ajuste para UTC-5 (Perú)
      parseInt(minutes)
    )
  );

  const formatter = new Intl.DateTimeFormat("es-PE", {
    day: "2-digit",
    month: "long",
    year: "numeric",
    hour: "2-digit",
    minute: "2-digit",
    hour12: false,
    timeZone: "America/Lima",
  });

  const nuevoMovimiento = {
    id: Date.now().toString(),
    name: nombre.value,
    type: tipo.value,
    date: formatter.format(fechaCompleta),
    isoDate: fechaCompleta.toISOString(),
    amount: monto.value,
  };

  emit("guardar", nuevoMovimiento);
  movimientos.value.push(nuevoMovimiento);

  if (salir) {
    router.push('/');
  } else {
    nombre.value = "";
    monto.value = null;
    tipo.value = route.query.tipo?.toString() || "Ingresos";
    
    const options: Intl.DateTimeFormatOptions = { 
      timeZone: 'America/Lima',
      year: 'numeric',
      month: '2-digit',
      day: '2-digit',
      hour: '2-digit',
      minute: '2-digit',
      hour12: false
    };
    
    const peruDateTime = new Date().toLocaleString('en-US', options);
    const [date, time] = peruDateTime.split(', ');
    const [month, day, year] = date.split('/');
    
    fecha.value = `${year}-${month}-${day}`;
    hora.value = time;
  }
}
</script>

<template>
  <div class="flex justify-center py-20">
    <fieldset
      class="fieldset w-xs bg-base-200 border border-base-300 p-4 rounded-box space-y-4"
    >
      <legend class="fieldset-legend font-semibold">Nuevo Movimiento</legend>

      <div class="flex items-center gap-2">
        <label class="w-24 text-left">Nombre</label>
        <input
          v-model="nombre"
          type="text"
          placeholder="Sueldo"
          class="input w-full"
        />
      </div>

      <div class="flex items-center gap-2">
        <label class="w-24 text-left">Monto</label>
        <input
          v-model="monto"
          type="number"
          placeholder="100"
          class="input w-full"
        />
      </div>

      <div class="flex items-center gap-2">
        <label class="w-24 text-left">Tipo</label>
        <select v-model="tipo" class="select w-full">
          <option>Ingresos</option>
          <option>Salidas</option>
        </select>
      </div>

      <div class="flex items-center gap-2">
        <label class="w-24 text-left">Fecha</label>
        <input v-model="fecha" type="date" class="input w-full" />
      </div>

      <div class="flex items-center gap-2">
        <label class="w-24 text-left">Hora</label>
        <input v-model="hora" type="time" class="input w-full" />
      </div>

      <button @click="() => guardar(true)" class="btn btn-dash btn-primary w-full">
        GUARDAR Y SALIR
      </button>

      <button @click="() => guardar(false)" class="btn btn-dash btn-secondary w-full">
        GUARDAR Y AGREGAR OTRO
      </button>
      
      <RouterLink to="/">
        <button class="btn btn-dash btn-error w-full">CANCELAR</button>
      </RouterLink>
    </fieldset>
  </div>
</template>
