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
  const ahoraEnPeru = new Date(
    new Date().toLocaleString("en-US", { timeZone: "America/Lima" })
  );

  fecha.value = ahoraEnPeru.toISOString().split("T")[0];

  const horas = ahoraEnPeru.getHours().toString().padStart(2, "0");
  const minutos = ahoraEnPeru.getMinutes().toString().padStart(2, "0");
  hora.value = `${horas}:${minutos}`;
});

function guardar(salir = false) {
  if (
    !nombre.value ||
    !monto.value ||
    !fecha.value ||
    !hora.value ||
    monto.value <= 0
  )
    return;

  const fechaCompleta = new Date(`${fecha.value}T${hora.value}`);

  const fechaEnPeru = new Date(
    fechaCompleta.toLocaleString("en-US", { timeZone: "America/Lima" })
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
    date: formatter.format(fechaEnPeru),
    isoDate: fechaEnPeru.toISOString(),
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
    
    const ahora = new Date(
      new Date().toLocaleString("en-US", { timeZone: "America/Lima" })
    );
    fecha.value = ahora.toISOString().split("T")[0];
    hora.value = `${ahora.getHours().toString().padStart(2, "0")}:${ahora
      .getMinutes()
      .toString()
      .padStart(2, "0")}`;
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
