# TalentoCine

Aplicación de selección de butacas para cine desarrollada con Vue 3 y Vite.

Este proyecto fue realizado como práctica final del módulo sobre reactividad en Vue, aplicando `reactive`, `ref`, `computed`, `watch` y `v-model` para gestionar la selección de asientos, los datos del cliente y las validaciones de compra.

## Vista general

TalentoCine permite:
- Seleccionar y deseleccionar butacas disponibles.
- Impedir la selección de butacas ocupadas.
- Mostrar en tiempo real las butacas elegidas.
- Calcular subtotal y total según el tipo de función.
- Ingresar el nombre de quien retira mediante `v-model`.
- Deshabilitar la compra si faltan datos o si se supera el máximo de 6 butacas.
- Mostrar una alerta cuando se seleccionan más de 6 asientos.

## Tecnologías utilizadas

- Vue 3
- Vite
- JavaScript
- HTML
- CSS

## Conceptos aplicados

### Rol A — Estado y butacas

En esta parte se resolvió la lógica relacionada con la sala y el cálculo del precio:

- `toggleAsiento(id)`: cambia una butaca entre `libre` y `elegido`.
- `elegidos`: computed que devuelve las butacas seleccionadas.
- `cantidad`: computed que cuenta cuántas butacas fueron elegidas.
- `subtotal`: computed que multiplica la cantidad por el precio base.
- `total`: computed que suma el recargo si la función es 3D.

### Rol B — Cliente y reglas

En esta parte se resolvió la lógica del comprador y las validaciones:

- `cliente.nombre`: enlazado con `v-model`.
- `tipoFuncion`: enlazado con `v-model`.
- `puedeComprar`: computed que valida nombre, butacas y límite máximo.
- `watch(cantidad)`: muestra un aviso cuando se seleccionan más de 6 butacas.
- `comprar()`: confirma la compra si se cumplen las condiciones.
- `comprado`: indica si la compra fue realizada.

## Estructura del proyecto

```txt
src/
├── assets/
├── components/
├── App.vue
├── main.js
└── style.css
```

- `App.vue`: contiene la estructura principal y toda la lógica reactiva de la aplicación.
- `style.css`: contiene los estilos globales de la interfaz.
- `main.js`: monta la aplicación Vue.
## Instalación y ejecución local

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/TU-USUARIO/TU-REPO.git
   ```

2. Entrar a la carpeta del proyecto:
   ```bash
   cd TU-REPO
   ```

3. Instalar dependencias:
   ```bash
   npm install
   ```

4. Ejecutar en desarrollo:
   ```bash
   npm run dev
   ```

5. Abrir en el navegador la URL que entrega Vite, normalmente:
   ```txt
   http://localhost:5173/
   ```

## Build de producción

Para generar la versión lista para publicar:

```bash
npm run build
```

Esto crea la carpeta `dist` con los archivos estáticos listos para despliegue en GitHub Pages.

## Publicación en GitHub Pages

Si el proyecto se publica como sitio de repositorio en GitHub Pages, Vite debe usar la opción `base` con el nombre exacto del repositorio para que los assets carguen correctamente desde la subruta del proyecto.

### 1. Configurar `vite.config.js`

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  base: '/nombre-del-repo/',
})
```

### 2. Instalar `gh-pages`

```bash
npm install gh-pages --save-dev
```

### 3. Configurar scripts en `package.json`

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

### 4. Ejecutar el deploy

```bash
npm run deploy
```

### 5. Activar GitHub Pages

En GitHub:
- Ir a **Settings**
- Entrar en **Pages**
- Elegir **Deploy from a branch**
- Seleccionar la rama `gh-pages`
- Seleccionar la carpeta `/ (root)`

## Reglas de funcionamiento

- Las butacas ocupadas no se pueden seleccionar.
- Las butacas libres pueden cambiar entre libre y elegida.
- El total cambia automáticamente si la función es 3D.
- El botón de compra solo se habilita si:
  - hay al menos una butaca seleccionada,
  - el nombre fue ingresado,
  - la cantidad seleccionada no supera 6.
- Si se seleccionan más de 6 butacas, aparece un aviso y la compra se bloquea hasta volver a 6 o menos.

## Mejoras incorporadas

Además de los requerimientos base del ejercicio, se agregaron algunas mejoras de experiencia:

- Botón para vaciar la selección.
- Mensaje dinámico en el botón de compra.
- Bloqueo automático de compra al superar el límite de butacas.
- Estilo visual inspirado en una interfaz de cine con panel lateral de resumen.

## Estado del proyecto

Proyecto funcional y preparado para seguir mejorando con nuevas características, por ejemplo:

- Mostrar detalle completo de la compra.
- Aplicar descuentos por cantidad.
- Indicar la fila con más asientos disponibles.
- Persistir compra o selección en almacenamiento/local backend.

## Puedes ver el resultado en:

https://zakkdruzer.github.io/m6-l3-d2-talentocine/
