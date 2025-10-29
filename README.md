# Sistema Solar 3D con Three.js

## Autores y asignatura
- **Autor**: Miguel Ángel Rodríguez Ruano
- **Universidad**: ULPGC - GII (Grado en Ingeniería Informática)
- **Asignatura**: Informática Gráfica

## Video de la demo
Aquí se muestra un video demostrativo de la aplicación.## 🎬 Video de la demo
[![Ver demo](https://img.youtube.com/vi/k1bgapVHT_w/0.jpg)](https://youtu.be/k1bgapVHT_w)


## Controles de la demo
- **Vista general**: Usa el mouse para orbitar, zoom y panear con los controles OrbitControls.
- **Cambio de vistas**:
  - Tecla 1: Vista general
  - Tecla 2: Venus
  - Tecla 3: Tierra
  - Tecla 4: Marte
  - Tecla 5: Júpiter
  - Tecla 6: Saturno
  - Tecla 7: Urano
  - Tecla 8: Neptuno
  - Tecla 9: Vehículo
- **Control del vehículo** (en vista "Vehículo"):
  - Flecha Abajo: Avanzar
  - Flecha Arriba: Retroceder
  - Flecha Izquierda: Girar izquierda
  - Flecha Derecha: Girar derecha
  - Espacio: Subir
  - Shift Izquierdo: Bajar
- **GUI (lil-gui)**:
  - Velocidad órbitas: Ajusta la velocidad de las órbitas (0.1 a 5).
  - Vista: Selecciona la vista del planeta o general.
  - Mostrar órbitas: Muestra/oculta las líneas de órbitas.
  - Modo añadir planetas: Activa el modo para añadir planetas con clic derecho en la vista general.
- **Añadir planetas**: En modo "addMode" activado, haz clic derecho en la vista general para añadir un planeta nuevo en esa posición.

## Explicación del código
Este proyecto es una simulación 3D interactiva del sistema solar desarrollada con Three.js, una biblioteca de JavaScript para renderizado 3D en el navegador. La simulación incluye planetas con órbitas elípticas, inclinaciones axiales y orbitales, rotaciones, lunas, anillos, cinturones de asteroides, un vehículo controlable y la capacidad de añadir planetas personalizados de forma dinámica. El código está escrito en JavaScript modular (usando importaciones ES6) y se ejecuta en un entorno web. A continuación, se explica el código de manera detallada, sección por sección, destacando las variables clave, funciones principales y lógica de implementación.

### 1. Importaciones y Configuración Inicial
El código comienza importando las dependencias necesarias:
- `* as THREE from "three"`: Importa la biblioteca principal de Three.js para crear escenas, meshes, cámaras, etc.
- `{ OrbitControls } from "three/examples/jsm/controls/OrbitControls.js"`: Controles para orbitar la cámara en la vista general.
- `{ GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js"`: Cargador para modelos 3D en formato GLB (usado para el vehículo).
- `GUI from "lil-gui"`: Biblioteca para crear una interfaz gráfica de usuario (GUI) simple para controles como velocidad y vistas.

Luego, se define un objeto `TEX` que contiene rutas a texturas para planetas, estrellas, asteroides y elementos personalizados (como `/src/random1.jpg` a `/src/random6.jpg` para planetas añadidos). Estas texturas se cargan dinámicamente para aplicar materiales realistas.

El array `BODIES` define los datos de los planetas predeterminados (Venus, Tierra, etc.), incluyendo:
- `name`: Nombre del planeta.
- `tex`: Clave de textura en `TEX`.
- `r`: Radio.
- `a` y `b`: Semiejes mayor y menor para órbitas elípticas.
- `s`: Velocidad orbital.
- `tilt`: Inclinación axial (en radianes).
- `rotSpeed`: Velocidad de rotación.
- `orbitTilt`: Inclinación orbital (aumentada ligeramente para más realismo, e.g., Venus a 5.1°).

Variables globales incluyen `scene`, `renderer`, cámaras (`camGen` para vista general, `camPlanet` para vistas planetarias), `controls`, `gui`, `clock` (para deltas de tiempo), arrays como `planets` y `meshes` para rastrear objetos, y parámetros de GUI en `params` (velocidad, helpers, vista, modo añadir).

### 2. Función `init()`
Esta función inicializa todo el entorno:
- Configura el estilo del body (sin márgenes, overflow hidden).
- Crea la `scene` y el `renderer` con antialiasing, sombras suaves (PCFSoftShadowMap), espacio de color sRGB y luces físicamente correctas.
- Inicializa cámaras: `camGen` con FOV 50 para vista amplia, `camPlanet` con FOV 60 para vistas cercanas.
- Configura `controls` (OrbitControls) con damping para movimientos suaves.
- Crea la GUI con sliders y checkboxes para `speed`, `view`, `helpers` y `addMode`.
- Carga texturas en un objeto `tx` usando `TextureLoader`, aplicando anisotropy para mejor calidad.
- Añade el fondo de estrellas: Una esfera grande (radio 20000 para mayor inmersión) con material básico y textura de estrellas, renderizada desde el interior (`side: THREE.BackSide`).
- Crea el Sol: Mesh esférico con textura emisiva (brillo propio) y añade una luz puntual central con sombras.
- Añade luz ambiental tenue.
- Para cada planeta en `BODIES`:
  - Crea un grupo (`grp`) rotado por `orbitTilt` para la inclinación orbital.
  - Crea el mesh esférico con material estándar (textura, roughness=1, metalness=0).
  - Activa sombras (cast/receive).
  - Crea y añade una órbita elíptica como línea (`createOrbit`).
  - Posiciona el mesh en una posición aleatoria en la órbita.
  - Añade al array `planets` y `meshes`.
- Características especiales:
  - Tierra: Añade bump map para relieve, nubes transparentes y una luna con órbita propia.
  - Venus: Añade atmósfera semitransparente.
  - Júpiter: Añade múltiples lunas con datos predefinidos.
  - Saturno: Añade anillos con textura transparente y un cinturón de debris (500 asteroides aleatorios con texturas de piedra, posiciones y rotaciones).
- Cinturón de asteroides principal: 1500 meshes dodecaédricos con texturas de piedra, posiciones aleatorias en un anillo entre Marte y Júpiter, con variación en Y para profundidad.
- UI: Añade un div con atajos de teclado.
- Eventos: Resize para ajustar cámaras y renderer; keydown para cambiar vistas.
- Vehículo: Carga modelo GLB (`bano_exterior.glb`), escala, añade luces puntuales en múltiples direcciones y configura controles con teclas (movimiento, rotación, vertical).
- Configuración para añadir planetas: Raycaster, mouse vector, plano invisible para intersecciones; evento mousedown para clic derecho.

### 3. Función `createOrbit(a, b)`
Genera una curva elíptica con `EllipseCurve`, convierte puntos a geometría buffer y crea una línea loop gris para visualizar órbitas.

### 4. Función `animate()`
Bucle principal (requestAnimationFrame):
- Calcula delta de tiempo (`dt`).
- Actualiza planetas: Incrementa ángulo orbital, recalcula posición (x = a*cos, z = b*sin), rota mesh en Y.
- Actualiza lunas: Copia posición del planeta padre, rota grupo orbital.
- Actualiza debris de Saturno y asteroides: Rotaciones orbitales y propias.
- Rotación extra para meshes esféricos (nubes, etc.).
- Vista de vehículo: Procesa teclas para movimiento/rotación, actualiza posición y cámara follow.
- Renderizado: Actualiza controls en vista general; en vistas planetarias, posiciona cámara relativa al planeta y mira al centro.
- Maneja vistas dinámicas para planetas añadidos.

### 5. Función `addNewPlanet(point)`
Activada por clic derecho en modo add:
- Calcula distancia orbital `a` desde el sol, excentricidad `ecc` (casi circular), `b = a*ecc`.
- Elige textura random de `/src/randomX.jpg`.
- Genera parámetros aleatorios: radio `r`, velocidad `s` (inversa a distancia), tilt, rotSpeed.
- Crea grupo, mesh, órbita similar a planetas default.
- Añade 0-2 lunas con probabilidad (70% ninguna, 20% una, 10% dos), usando textura de luna estándar.
- Añade a `planets`, `meshes`, actualiza GUI con nueva vista.

### Consideraciones Generales
- **Rendimiento**: Usa geometrías de baja resolución para lunas/asteroides; filtra texturas válidas para evitar errores.
- **Realismo**: Inclinaciones orbitales aumentadas (e.g., Marte a 2.775°); sombras y luces para profundidad.
- **Interactividad**: Modo añadir permite expansión dinámica; GUI y teclas para control intuitivo.
- **Ejecución**: Requiere servidor local (e.g., `npx serve`) por CORS en texturas/modelos. No hay dependencias externas más allá de las importadas.

Este código es modular y extensible, ideal para proyectos educativos en gráfica 3D.