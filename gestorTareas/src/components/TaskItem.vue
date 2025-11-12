<template>
  <!-- Representa una tarea individual -->
  <div class="elemento">
    <!-- El título se tacha si está completada -->
    <span class="titulo" :class="{ completada: tarea.estado === 'completada' }">
      {{ tarea.titulo }}
    </span>

    <!-- Botones de acción -->
    <div class="acciones">
      <!-- Cambia el estado entre pendiente y completada -->
      <button
        class="boton"
        @click="$emit('cambiar-estado')"
        title="Completar o reabrir"
      >
        {{ tarea.estado === 'pendiente' ? 'Completar' : 'Reabrir' }}
      </button>

      <!-- Elimina la tarea -->
      <button
        class="boton peligro"
        @click="$emit('eliminar')"
        title="Eliminar tarea"
      >
        Eliminar
      </button>
    </div>
  </div>
</template>

<script setup>
// Recibo la tarea individual del padre
defineProps({
  tarea: {
    type: Object,
    required: true
  }
})

// Declaro los eventos que este componente puede emitir
defineEmits(['cambiar-estado', 'eliminar'])
</script>

<style scoped>
.elemento {
  display: flex;
  align-items: center;
  gap: .75rem;
  background-color: #2a2a2a;
  padding: 10px 12px;
  border-radius: 6px;
}

.titulo {
  flex: 1;
  text-align: left;
  font-size: 1rem;
  color: #00b894;
}

.completada {
  text-decoration: line-through;
  color: #888;
}

.acciones {
  display: flex;
  gap: .5rem;
}

.boton {
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  background-color: #3b82f6;
  transition: background .2s, opacity .2s;
}

.boton:hover { background-color: #2563eb; }

.peligro { background-color: #e11d48; }
.peligro:hover { background-color: #be123c; }
</style>
