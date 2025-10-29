# Sistema Solar 3D con Three.js

## Autores y asignatura
- **Autor**: Miguel Ángel Rodríguez Ruano
- **Universidad**: ULPGC - GII (Grado en Ingeniería Informática)
- **Asignatura**: Informática Gráfica

## Video de la demo
Aquí se muestra un video demostrativo de la aplicación. (Nota: Reemplaza el enlace con el video real si está disponible, o usa un GIF animado).

![Demo GIF](./Demo.mp4))

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
Este proyecto es una simulación 3D del sistema solar utilizando Three.js, con planetas orbitando el sol, lunas, anillos, cinturones de asteroides y un vehículo controlable. El código está estructurado en JavaScript con importaciones de módulos Three.js y otras bibliotecas.

### Estructura principal:
- **Configuración de texturas (TEX)**: Un objeto que define rutas a texturas para planetas, estrellas, etc.
- **Definición de cuerpos celestes (BODIES)**: Array con datos de cada planeta (radio, semiejes de órbita, velocidad, inclinación, etc.).
- **Inicialización (init())**:
  - Crea la escena, renderer, cámaras (general y planetaria).
  - Configura controles OrbitControls y GUI con lil-gui.
  - Carga texturas y crea el fondo de estrellas (esfera grande con textura).
  - Crea el sol y la luz puntual.
  - Genera planetas con meshes, grupos, órbitas elípticas y helpers.
  - Añade características especiales: nubes y luna para Tierra, atmósfera para Venus, lunas para Júpiter, anillos y cinturón para Saturno.
  - Crea el cinturón de asteroides principal.
  - Carga un modelo GLB para el vehículo y añade luces.
  - Configura eventos para resize, keydown/keyup y mousedown (para añadir planetas).
- **Función createOrbit()**: Genera una línea elíptica para las órbitas.
- **Animación (animate())**:
  - Actualiza posiciones de planetas, lunas, debris y asteroides basado en tiempo delta.
  - Maneja rotaciones y movimientos.
  - Actualiza el vehículo si en esa vista.
  - Renderiza según la vista seleccionada (general o específica de planeta).
- **Añadir planetas nuevos (addNewPlanet())**:
  - Calcula parámetros aleatorios (textura random, radio, velocidad, etc.).
  - Crea mesh, grupo y órbita.
  - Añade hasta 2 lunas con probabilidad.
  - Actualiza opciones de GUI para vistas.
