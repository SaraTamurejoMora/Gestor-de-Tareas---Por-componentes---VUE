<template>
  <header>
    <h1>Gestor de Tareas</h1>
  </header>

  <main>
    <div class="tarjeta">
      <h2>Tareas</h2>

      <!-- El formulario emite el texto de la tarea y se añade directamente -->
      <TaskForm
        @añadir="texto => {
          const limpio = (texto ?? '').trim()
          if (!limpio) return
          misTareas.push({ titulo: limpio, estado: 'pendiente' })
        }"
      />

      <!-- Filtro: si está marcado, solo se muestran las pendientes -->
      <div class="fila-filtro">
        <input type="checkbox" v-model="soloPendientes" id="soloPendientes" />
        <label for="soloPendientes">Mostrar solo pendientes</label>
      </div>

      <!-- Paso las tareas filtradas al componente hijo y controlo los emits -->
      <TaskList
        :tareas="tareasMostradas"
        @cambiar-estado="tarea => {
          tarea.estado = tarea.estado === 'pendiente' ? 'completada' : 'pendiente'
        }"
        @eliminar="tarea => {
          misTareas = misTareas.filter(t => t !== tarea)
        }"
      />

      <!-- Contadores de tareas -->
      <div class="totales">
        <p>Total: {{ total }}</p>
        <span> | Pendientes: {{ pendientes }}</span>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskList from './components/TaskList.vue'

// Estado principal: lista de tareas
const misTareas = ref([
  { titulo: 'Acabar proyecto VUE', estado: 'pendiente' },
  { titulo: 'Estudiar examen REACT', estado: 'pendiente' },
  { titulo: 'Subir blog inglés', estado: 'completada' },
  { titulo: 'Hacer práctica de redes', estado: 'completada' }
])

// Control del filtro
const soloPendientes = ref(false)

// Cálculo de tareas filtradas según el checkbox
const tareasMostradas = computed(() =>
  soloPendientes.value
    ? misTareas.value.filter(t => t.estado === 'pendiente')
    : misTareas.value
)

// Contadores
const total = computed(() => misTareas.value.length)
const pendientes = computed(() => misTareas.value.filter(t => t.estado === 'pendiente').length)
</script>

<style scoped>
body {
  background-color: #121212;
  font-family: 'Poppins', sans-serif;
  color: #e0e0e0;
  margin: 0;
  padding: 0;
}

header {
  background-color: #1f1f1f;
  padding: 1rem;
  text-align: center;
  color: #ffffff;
  border-bottom: 2px solid #333;
}

main {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}

.tarjeta {
  background-color: #1e1e1e;
  border-radius: 10px;
  padding: 1.5rem;
  width: 100%;
  max-width: 560px;
  box-shadow: 0 0 10px rgba(0,0,0,0.3);
}

h2 {
  color: #81a1c1;
  border-bottom: 1px solid #333;
  padding-bottom: 6px;
  margin-bottom: 10px;
  font-weight: 500;
}

.fila-filtro {
  display: flex;
  align-items: center;
  gap: .5rem;
  margin: .5rem 0 1rem;
}

input[type="checkbox"] { accent-color: #81a1c1; }
label { color: #ccc; }

.totales {
  margin-top: .75rem;
  color: #ffffff;
}
.totales p {
  display: inline;
  font-weight: 500;
}
</style>
