<template>
  <div class="lista">
    <!-- Recorro todas las tareas que me llegan del padre -->
    <!-- Uso índice porque no hay id -->
    <!-- Reenvío los eventos al componente padre -->
    <TaskItem
      v-for="(tarea, indice) in tareas"
      :key="indice"
      :tarea="tarea"
      @cambiar-estado="$emit('cambiar-estado', tarea)"
      @eliminar="$emit('eliminar', tarea)"
    />

    <!-- Mensaje cuando no hay tareas -->
    <p v-if="!tareas?.length" class="vacio">No hay tareas.</p>
  </div>
</template>


<script setup>
import TaskItem from './TaskItem.vue'

// Recibo la lista de tareas del padre
defineProps({
  tareas: {
    type: Array,
    required: true
  }
})

// Declaro los eventos que se van a reemitir
defineEmits(['cambiar-estado', 'eliminar'])
</script>

<style scoped>
.lista {
  display: flex;
  flex-direction: column;
  gap: .5rem;
}

.vacio {
  opacity: .7;
  margin: .5rem 0;
}
</style>
