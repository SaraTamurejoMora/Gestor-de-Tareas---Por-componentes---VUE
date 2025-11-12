<template>
  <!-- Formulario para añadir una nueva tarea -->
  <form @submit.prevent="enviarFormulario" class="formulario">
    <input
      v-model="textoTarea"
      type="text"
      placeholder="Nombre de la tarea"
      class="entrada"
    />
    <button type="submit" class="boton primario" :disabled="!tieneContenido">
      Añadir
    </button>
  </form>
</template>

<script setup>
import { ref, computed } from 'vue'

// Declaro el evento que este componente emitirá al padre
const emitir = defineEmits(['añadir'])

// Campo del input controlado
const textoTarea = ref('')

// Computed para activar/desactivar el botón
const tieneContenido = computed(() => textoTarea.value.trim().length > 0)

// Envía el texto limpio al padre y resetea el campo
const enviarFormulario = () => {
  const limpio = textoTarea.value.trim()
  if (!limpio) return
  emitir('añadir', limpio)
  textoTarea.value = ''
}
</script>

<style scoped>
.formulario {
  display: flex;
  gap: .5rem;
  margin-bottom: .5rem;
}

.entrada {
  flex: 1;
  padding: 8px;
  border: 1px solid #444;
  border-radius: 6px;
  background-color: #2a2a2a;
  color: #f1f1f1;
}

.entrada:focus {
  outline: none;
  border-color: #81a1c1;
}

.boton {
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  transition: background .2s, opacity .2s;
}

.primario { background-color: #00b894; }
.primario:hover { background-color: #00a383; }

.boton:disabled {
  opacity: .6;
  cursor: not-allowed;
}
</style>
