<script setup>
import { reactive, ref, computed, watch } from 'vue'

/* =========================================================
   CONSTANTES DEL EJERCICIO
   =========================================================
   Se usan para calcular subtotal y total.
========================================================= */
const PRECIO_BASE = 4500
const RECARGO_3D = 1500

/* =========================================================
   ESTADO COMPARTIDO
   =========================================================
   Este estado lo pide el enunciado:
   - asientos: array reactivo con id, fila, col y estado
   - cliente.nombre: nombre de quien retira
   - tipoFuncion: normal o 3d
========================================================= */
const asientos = reactive([
  { id: 'A1', fila: 'A', col: 1, estado: 'libre' },
  { id: 'A2', fila: 'A', col: 2, estado: 'libre' },
  { id: 'A3', fila: 'A', col: 3, estado: 'ocupado' },
  { id: 'A4', fila: 'A', col: 4, estado: 'ocupado' },
  { id: 'A5', fila: 'A', col: 5, estado: 'libre' },
  { id: 'A6', fila: 'A', col: 6, estado: 'libre' },
  { id: 'A7', fila: 'A', col: 7, estado: 'libre' },
  { id: 'A8', fila: 'A', col: 8, estado: 'libre' },

  { id: 'B1', fila: 'B', col: 1, estado: 'libre' },
  { id: 'B2', fila: 'B', col: 2, estado: 'libre' },
  { id: 'B3', fila: 'B', col: 3, estado: 'libre' },
  { id: 'B4', fila: 'B', col: 4, estado: 'libre' },
  { id: 'B5', fila: 'B', col: 5, estado: 'libre' },
  { id: 'B6', fila: 'B', col: 6, estado: 'libre' },
  { id: 'B7', fila: 'B', col: 7, estado: 'libre' },
  { id: 'B8', fila: 'B', col: 8, estado: 'libre' },

  { id: 'C1', fila: 'C', col: 1, estado: 'libre' },
  { id: 'C2', fila: 'C', col: 2, estado: 'libre' },
  { id: 'C3', fila: 'C', col: 3, estado: 'libre' },
  { id: 'C4', fila: 'C', col: 4, estado: 'libre' },
  { id: 'C5', fila: 'C', col: 5, estado: 'ocupado' },
  { id: 'C6', fila: 'C', col: 6, estado: 'libre' },
  { id: 'C7', fila: 'C', col: 7, estado: 'libre' },
  { id: 'C8', fila: 'C', col: 8, estado: 'libre' },

  { id: 'D1', fila: 'D', col: 1, estado: 'ocupado' },
  { id: 'D2', fila: 'D', col: 2, estado: 'ocupado' },
  { id: 'D3', fila: 'D', col: 3, estado: 'libre' },
  { id: 'D4', fila: 'D', col: 4, estado: 'libre' },
  { id: 'D5', fila: 'D', col: 5, estado: 'libre' },
  { id: 'D6', fila: 'D', col: 6, estado: 'libre' },
  { id: 'D7', fila: 'D', col: 7, estado: 'libre' },
  { id: 'D8', fila: 'D', col: 8, estado: 'libre' },

  { id: 'E1', fila: 'E', col: 1, estado: 'libre' },
  { id: 'E2', fila: 'E', col: 2, estado: 'libre' },
  { id: 'E3', fila: 'E', col: 3, estado: 'libre' },
  { id: 'E4', fila: 'E', col: 4, estado: 'libre' },
  { id: 'E5', fila: 'E', col: 5, estado: 'libre' },
  { id: 'E6', fila: 'E', col: 6, estado: 'libre' },
  { id: 'E7', fila: 'E', col: 7, estado: 'ocupado' },
  { id: 'E8', fila: 'E', col: 8, estado: 'libre' }
])

const cliente = reactive({
  nombre: ''
})

const tipoFuncion = ref('normal')

/* =========================================================
   ROL B - ESTADO EXTRA
   =========================================================
   - comprado: indica si la compra fue confirmada
   - mostrarAvisoLimite: muestra el aviso si superan 6 butacas
========================================================= */
const comprado = ref(false)
const mostrarAvisoLimite = ref(false)

/* =========================================================
   APOYO VISUAL
   =========================================================
   Agrupa los asientos por fila para dibujar el mapa
   fácilmente en el template.
========================================================= */
const filasAgrupadas = computed(() => {
  const letras = ['A', 'B', 'C', 'D', 'E']

  return letras.map(letra => ({
    letra,
    asientos: asientos.filter(asiento => asiento.fila === letra)
  }))
})

/* =========================================================
   ROL A - FUNCIÓN PRINCIPAL
   =========================================================
   toggleAsiento(id):
   - Busca el asiento por id
   - Si está ocupado, no hace nada
   - Si está libre, lo marca como elegido
   - Si está elegido, lo vuelve a dejar libre
========================================================= */
function toggleAsiento(id) {
  const asiento = asientos.find(a => a.id === id)

  if (!asiento) return
  if (asiento.estado === 'ocupado') return

  if (asiento.estado === 'libre') {
    asiento.estado = 'elegido'
  } else if (asiento.estado === 'elegido') {
    asiento.estado = 'libre'
  }

  // Si cambia la selección, se resetea el estado de compra
  comprado.value = false
}

/* =========================================================
   ROL A - COMPUTED
   =========================================================
   Valores derivados del estado de asientos.
   computed es ideal para esto porque Vue recalcula
   automáticamente cuando cambian las dependencias.
========================================================= */

// Lista de asientos que están en estado "elegido"
const elegidos = computed(() => {
  return asientos.filter(asiento => asiento.estado === 'elegido')
})

// Cantidad de butacas seleccionadas
const cantidad = computed(() => {
  return elegidos.value.length
})

// Subtotal = cantidad * precio base
const subtotal = computed(() => {
  return cantidad.value * PRECIO_BASE
})

// Total = subtotal + recargo si la función es 3D
const total = computed(() => {
  const recargo = tipoFuncion.value === '3d'
    ? cantidad.value * RECARGO_3D
    : 0

  return subtotal.value + recargo
})

/* =========================================================
   ROL B - VALIDACIONES
   =========================================================
   puedeComprar:
   - Debe haber al menos 1 butaca elegida
   - Debe haber nombre ingresado
   - Si hay más de 6 butacas, NO se puede comprar
========================================================= */
const puedeComprar = computed(() => {
  return (
    cantidad.value > 0 &&
    cantidad.value <= 6 &&
    cliente.nombre.trim().length > 0
  )
})

/* =========================================================
   ROL B - WATCH
   =========================================================
   Observa la cantidad de butacas elegidas.
   Si el usuario selecciona más de 6, se muestra un aviso.
========================================================= */
watch(cantidad, (nuevaCantidad) => {
  mostrarAvisoLimite.value = nuevaCantidad > 6
})

/* =========================================================
   ROL B - ACCIÓN DE COMPRA
   =========================================================
   comprar():
   - Solo confirma la compra si puedeComprar es true
========================================================= */
function comprar() {
  if (!puedeComprar.value) return
  comprado.value = true
}

/* =========================================================
   BONUS
   =========================================================
   Vacía todas las butacas elegidas y reinicia la compra.
========================================================= */
function vaciarSeleccion() {
  asientos.forEach(asiento => {
    if (asiento.estado === 'elegido') {
      asiento.estado = 'libre'
    }
  })

  comprado.value = false
}

/* =========================================================
   APOYO PARA LA UI
   =========================================================
   mensajeBoton cambia según el estado actual:
   - Sin nombre o sin butacas: pide completar datos
   - Más de 6 butacas: avisa el límite
   - Caso válido: muestra "Comprar"
========================================================= */
const mensajeBoton = computed(() => {
  if (cantidad.value === 0 || cliente.nombre.trim().length === 0) {
    return 'Elige butacas y escribe tu nombre'
  }

  if (cantidad.value > 6) {
    return 'Máximo 6 butacas'
  }

  return 'Comprar'
})
</script>

<template>
  <div class="app">
    <div class="cine">
      <section class="sala">
        <div class="pantalla">PANTALLA</div>

        <div class="mapa">
          <div
            v-for="fila in filasAgrupadas"
            :key="fila.letra"
            class="fila"
          >
            <div class="fila-label">{{ fila.letra }}</div>

            <button
              v-for="asiento in fila.asientos"
              :key="asiento.id"
              class="asiento"
              :class="{
                'asiento--libre': asiento.estado === 'libre',
                'asiento--elegido': asiento.estado === 'elegido',
                'asiento--ocupado': asiento.estado === 'ocupado'
              }"
              :disabled="asiento.estado === 'ocupado'"
              @click="toggleAsiento(asiento.id)"
            >
              {{ asiento.col }}
            </button>
          </div>
        </div>

        <div class="leyenda">
          <div class="leyenda-item">
            <span class="muestra muestra--libre"></span>
            <span>Libre</span>
          </div>

          <div class="leyenda-item">
            <span class="muestra muestra--elegido"></span>
            <span>Elegido</span>
          </div>

          <div class="leyenda-item">
            <span class="muestra muestra--ocupado"></span>
            <span>Ocupado</span>
          </div>
        </div>
      </section>

      <aside class="panel">
        <h2>🎟️ Tu compra</h2>

        <p class="subtitulo">Butacas elegidas:</p>

        <p v-if="cantidad === 0" class="texto-suave">
          Ninguna todavía
        </p>

        <div v-else class="chips">
          <span
            v-for="asiento in elegidos"
            :key="asiento.id"
            class="chip"
          >
            {{ asiento.id }}
          </span>
        </div>

        <label for="tipoFuncion">Tipo de función</label>
        <select id="tipoFuncion" v-model="tipoFuncion">
          <option value="normal">2D — normal</option>
          <option value="3d">3D — (+$1.500 por butaca)</option>
        </select>

        <label for="nombre">Nombre de quien retira *</label>
        <input
          id="nombre"
          v-model.trim="cliente.nombre"
          type="text"
          placeholder="Tu nombre"
        >

        <div class="resumen-linea">
          <span>Butacas ({{ cantidad }})</span>
          <span class="precio">${{ subtotal.toLocaleString('es-CL') }}</span>
        </div>

        <div class="total-linea">
          <span>Total</span>
          <span class="precio-total">${{ total.toLocaleString('es-CL') }}</span>
        </div>

        <button
          class="btn-comprar"
          :disabled="!puedeComprar"
          @click="comprar"
        >
          {{ mensajeBoton }}
        </button>

        <button
          class="btn-vaciar"
          :disabled="cantidad === 0"
          @click="vaciarSeleccion"
        >
          Vaciar selección
        </button>

        <div v-if="mostrarAvisoLimite" class="aviso">
          Elegiste más de 6 butacas.
        </div>

        <div v-if="comprado" class="exito">
          Compra lista para {{ cliente.nombre }}.
        </div>
      </aside>
    </div>
  </div>
</template>