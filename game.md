# RutaMax Bogotá: Entregas a Tiempo — Prompt de construcción

## Rol
Actúa como un game designer senior y desarrollador frontend experto en juegos web,
especialista en "game feel", renderizado en Canvas y sistemas de IA para videojuegos.
Tu tarea es construir un juego completo y jugable en el navegador llamado
**"RutaMax Bogotá: Entregas a Tiempo"**, siguiendo este documento.

---

## 0. Prompt base (plantilla Claude for Gamers)
```
Crea un juego de arcade de entregas contrarreloj (top-down 2.5D) en un solo
archivo HTML, sin librerías ni imágenes externas.
Concepto: un repartidor rolo entrega paquetes contrarreloj por Bogotá mientras
un Copiloto IA tipo Waze le planea la ruta óptima.
Core loop: el jugador conduce y elige rutas para entregar todos los pedidos
antes de que se acabe el tiempo, mientras esquiva huecos, trancones, aguaceros
y direcciones erradas.
Controles: flechas/WASD para moverse, Espacio para Nitro, 1/2/3 o clic para
elegir ruta, P para pausa y R para reintentar; joystick táctil en mobile.
Gana si cumple la meta de entregas del turno (5 en Chapinero) antes de 180 s.
Pierde si el reloj llega a 0 sin cumplir la meta, o si la integridad de los
paquetes o la batería llegan a 0%.
Estilo: flat redondeado inspirado en e-commerce (amarillo #FFE600 y azul
#3483FA con toques bogotanos), sonidos generados en código con Web Audio.
Game feel: screen shake, partículas y easing en el movimiento, hit-stop en los
huecos, cámara con look-ahead y zoom con el Nitro.
Incluye pantalla de inicio, puntaje y reinicio rápido (menos de 1 s).
```

Todo lo que sigue es el **detalle ampliado** de cada línea de esta plantilla:

| Línea de la plantilla | Dónde se detalla |
|---|---|
| Género y restricciones | Secciones 1 y 14 |
| Concepto | Secciones 1, 3 y 5 |
| Core loop | Sección 4 (y 6 para los obstáculos) |
| Controles | Sección 14.1 |
| Gana / Pierde | Sección 14.2 |
| Estilo y sonido | Secciones 2, 3.4 y 10 |
| Game feel | Secciones 11 y 13 |
| Inicio, puntaje y reinicio rápido | Sección 12 y 11.2 |

---

## 1. Concepto
Arcade de entregas con vista cenital (top-down 2.5D) en Bogotá, Colombia.
El jugador es un repartidor rolo que tiene que entregar paquetes antes de que se acabe
el tiempo, esquivando huecos, trancones, lluvia y direcciones imposibles.
Lo ayuda un **Copiloto con IA** que le propone la mejor ruta, al estilo de Waze.

Referencias de sensación de juego:
- **Crazy Taxi:** urgencia y combos.
- **Subway Surfers:** monedas, power-ups, misiones diarias.
- **Mini Motorways:** lectura clara del mapa.
- **Waze:** rutas en vivo y reportes de la comunidad.

---

## 2. Identidad visual (inspirada en Mercado Libre + Bogotá)
- **Paleta:** amarillo `#FFE600` (protagonista), azul `#3483FA` (UI y acciones),
  blanco, gris `#EDEDED`, verde `#00A650` (éxito), rojo `#F23D4F` (peligro).
- **Toques bogotanos:** gris lluvioso del cielo, ladrillo rojo de las fachadas,
  verde de los cerros orientales.
- Estilo flat, amable y redondeado, con cajas de cartón con cinta amarilla.
- Tipografía del sistema con aspecto redondeado
  (`ui-rounded, "SF Pro Rounded", "Segoe UI", system-ui, sans-serif`). No cargues fuentes externas.
- **NO uses logos ni marcas registradas oficiales:** crea íconos y logo propios.
- UI tipo app de e-commerce moderna: tarjetas, badges, barras de progreso y
  microanimaciones.
- Los cerros orientales siempre se ven al oriente del mapa y sirven de brújula
  natural: "los cerros quedan al oriente".

---

## 3. Bogotá como protagonista

### 3.1 Mapa
- Ciudad en grilla con la nomenclatura real bogotana: las **calles** aumentan hacia
  el norte y las **carreras** hacia el occidente.
- Direcciones con formato real: `Calle 72 # 10-34, Torre 2, Apto 502`.
- Avenidas principales estilizadas: Carrera Séptima, Av. Caracas, NQS, Av. Boyacá,
  Calle 26, Calle 80, Autopista Norte.
- Landmarks icónicos: Monserrate, Plaza de Bolívar, Torre Colpatria,
  Parque Simón Bolívar, Museo del Oro, Estadio El Campín.

### 3.2 Barrios desbloqueables (cada uno con su personalidad y su reto)
| Barrio | Personalidad | Reto |
|---|---|---|
| Chapinero | Edificios y porterías | Tráfico denso, mucho Express |
| La Candelaria | Empedrado, graffitis, turistas | Calles estrechas y subidas |
| Teusaquillo | Casas de ladrillo, árboles | Calles tranquilas pero confusas |
| Usaquén | Calles coloniales, mercado de pulgas | Domingos llenos de gente |
| Zona T / Zona Rosa | Vida nocturna | Modo noche y mucho peatón |
| Salitre | Avenidas anchas, conjuntos | Porterías y conjuntos laberinto |
| Kennedy y Suba | Barrios enormes | Direcciones con Bis, Sur y letras (`Calle 38 Sur # 72B-15`) |

### 3.3 Mecánicas bogotanas
- 🚌 **Carril exclusivo de TransMilenio:** vas más rápido, pero te arriesgas a una
  multa (puede aparecer un agente de tránsito).
- 🚦 **Pico y placa:** ciertos días del juego tu vehículo queda restringido y la IA
  te sugiere bici o moto.
- 🚴 **Ciclovía dominical:** algunas avenidas se cierran a los carros; en bici
  vuelas y ganas bonus.
- 🌧️ **Clima bogotano:** sol en la mañana, aguacero a las 3 p. m., granizada
  sorpresa y neblina en la noche. "Las cuatro estaciones en un solo día."
- 🏔️ **Subidas hacia los cerros:** consumen más batería.

### 3.4 Cultura y humor rolo
- Clientes y NPC que hablan como bogotanos:
  - "Sumercé, déjelo con el portero."
  - "¡Uy, qué pena con usted, veci!"
  - "¡Qué chimba, llegó rapidísimo!"
  - "Parce, es el conjunto de la reja verde."
- Comida bogotana como power-ups (sección 8).
- Música con aire andino-urbano y sonidos de ciudad: pitos, vendedores ambulantes, lluvia.

---

## 4. Loop principal
1. Recibes un lote de pedidos (de 1 a 5) con dirección bogotana, ventana de tiempo y
   tipo: **Normal, Express, Frágil, Full/Prioritario**.
2. El Copiloto IA te propone de 2 a 3 rutas.
3. Conduces, esquivas obstáculos y recoges monedas **Plus** y power-ups.
4. Entregas: puntaje según el tiempo, el estado del paquete y la calificación del
   cliente (de 1 a 5 estrellas).
5. Gastas los Plus en el Garaje para mejorar el vehículo y desbloquear barrios.

---

## 5. Copiloto IA (la funcionalidad estrella)
Panel lateral tipo Waze con un asistente de voz rola que "piensa en voz alta".

- **Planificación multi-entrega:** ordena las paradas resolviendo un VRP con
  ventanas de tiempo (heurística vecino más cercano + optimización 2-opt).
  Muestra el orden sugerido y el ETA de cada parada.
- **Pathfinding:** A* sobre el grafo de calles con pesos dinámicos (trancón, huecos,
  cierres, lluvia, pico y placa, carril de TransMilenio).
- **3 perfiles de ruta:**
  - ⚡ **Rápida:** más riesgo.
  - 🛡️ **Segura:** menos huecos.
  - 🌱 **Eco/Ciclovía:** bonus de Plus.
- **Explicabilidad:** cada sugerencia trae su razón:
  "Evito la Boyacá: trancón de 12 min y 3 huecos reportados. Me voy por la 68,
  te ahorro 40 s."
- **Re-ruteo en vivo:** si aparece un obstáculo, recalcula y muestra
  "Recalculando, veci…".
- **Reportes de la comunidad:** repartidores NPC reportan huecos, accidentes y
  retenes; el jugador también reporta con un botón y gana Plus.
- **Mapa de calor predictivo:** zonas de riesgo según la hora del día (hora pico de
  6–9 a. m. y de 5–8 p. m., el aguacero de la tarde, la noche).
- **Aprendizaje:** la IA registra qué rutas te funcionan y ajusta sus sugerencias:
  "Noté que prefieres la Séptima; ajusté tu perfil."
- **Confianza:** cada sugerencia muestra un % de confianza. A veces la IA se
  equivoca a propósito para que el jugador tenga que decidir.

---

## 6. Obstáculos y eventos
- 🕳️ **Huecos:** frenan y dañan el paquete (los Frágiles pierden estrellas). Algunos
  son "huecos legendarios" con nombre propio y aparecen en el ranking.
- 📍 **Dirección errada:** minijuego rápido para encontrar el lugar correcto con
  pistas bogotanas ("la casa esquinera al lado de la panadería, frente a la tienda de
  don Jairo"). Incluye confusiones típicas: Bis, Sur, letras A/B/C, "Interior 4".
- 🏢 **Portería del conjunto / cliente ausente:** decides si esperas, se lo dejas al
  portero o reagendas.
- 🚧 Obras del metro y cierres de vía.
- 🚗 Trancones.
- 🌧️ Aguacero (menos agarre), ❄️ granizada (visión y control reducidos),
  🌫️ neblina.
- 🐕 **Perritos callejeros que cruzan:** si frenas a tiempo ganas el bonus
  "Corazón de oro" (+Plus).
- 🚓 Retenes de tránsito, 🛒 vendedores ambulantes y bicitaxis que se atraviesan.
- 🔋 Batería o gasolina limitada (las subidas consumen más).

---

## 7. Economía: monedas "Plus"
- **Se ganan por:** entregas a tiempo, combos de entregas seguidas sin daño,
  reportes útiles, rutas Eco/Ciclovía, misiones diarias y racha de días jugados.
- **Se gastan en:** vehículos, mejoras, skins y power-ups.
- El balance se muestra en el HUD con una animación de "lluvia de monedas".

---

## 8. Power-ups (con sabor bogotano)
| Power-up | Efecto |
|---|---|
| ☕ Tinto Nitro | Velocidad x2 durante 3 s (barra que se carga con combos) |
| 🥣 Ajiaco Escudo | Inmune a huecos durante 10 s |
| 🧲 Imán de Plus | Atrae las monedas cercanas |
| 🍫 Chocolate con queso | +15 s al pedido actual |
| 🥐 Almojábana GPS | La IA revela atajos ocultos del barrio |
| ☂️ Paraguas Pro | Anula los efectos del aguacero durante 15 s |
| 📦 Caja Express | Entrega instantánea cuando estás a menos de 50 m |

---

## 9. Progresión
- **Vehículos:** Bici → Moto → Furgoneta eléctrica → Dron (desbloqueable final:
  vuela sobre los trancones, pero tiene poca batería y sufre con el viento de los cerros).
- **Niveles de repartidor:** Bronce → Plata → Oro → Platino → "Leyenda Rola".
- **Barrios desbloqueables** con dificultad creciente (sección 3.2).
- **Misiones diarias y semanales:** "Entrega 5 Frágiles en La Candelaria sin daño",
  "Usa la ciclovía 3 veces".

---

## 10. Momentos "WOW"
- **Evento Hot Sale / Black Friday:** pedidos x3, música más rápida y multiplicador
  de Plus.
- **"Pedido Jefe: Monserrate":** una entrega Full que cruza la ciudad y termina
  subiendo a Monserrate, con obstáculos encadenados, cambio de clima y una
  cinemática final con la vista de Bogotá de noche.
- **Feedback del cliente:** mensajes rolos y graciosos al entregar.
- **Replay de la ruta:** al terminar, un mapa compara tu ruta con la óptima de la IA:
  "Eficiencia: 87%. Perdiste 22 s en la Caracas."
- **Ranking semanal** local y logros con badges: "Sobreviví a la Boyacá",
  "Rey de la ciclovía", "Cero huecos".
- **Sonido** con Web Audio API (generado por código): moneda, nitro, hueco, pito,
  lluvia, entrega ok.

---

## 11. Diseño del reto: "una partida más" (enganche sano)
El objetivo es que el jugador quiera volver porque siente que **puede hacerlo mejor**,
no porque el juego lo manipule. Tensión, superación y maestría; nada de vicio.

### 11.1 Tensión que se siente en el cuerpo
- **Reloj vivo:** en los últimos 10 s el temporizador se pone rojo, late, la música
  sube de tempo y se oye un tic-tac. Una entrega lograda con menos de 1 s activa el
  bonus "¡Por un pelo!" (+Plus, cámara lenta y sonido épico).
- **Near-miss:** esquivar un hueco o un bus por centímetros da "¡Uy, casi!" y
  carga la barra de Nitro. Recompensa el riesgo bien tomado.
- **Riesgo vs. recompensa en cada decisión:** ¿me meto al carril de TransMilenio?,
  ¿sigo la ruta Rápida o la Segura?, ¿gasto el Tinto ahora o lo guardo?
- **Combo frágil:** el multiplicador crece con cada entrega perfecta y se pierde con
  un solo golpe. Mientras más alto, más se siente lo que está en juego.

### 11.2 Maestría visible ("sé que puedo hacerlo mejor")
- **3 estrellas por turno** con metas claras (tiempo, cero daño, eficiencia frente a
  la IA). Al terminar, el resumen muestra qué faltó: "Te faltaron 4 s para la 3.ª estrella".
- **Fantasma (ghost):** tu mejor intento corre a tu lado como silueta translúcida.
- **"Vence a la IA":** reto opcional de lograr una ruta más eficiente que la
  sugerida por el Copiloto. Si lo logras: badge "Más pilo que la IA".
- **Récord personal destacado** en cada barrio, con animación cuando lo superas.
- **Reintento instantáneo:** un botón (o la tecla R) y vuelves a arrancar en menos
  de 1 s, sin pantallas de carga. Los turnos duran de 2 a 3 minutos.

### 11.3 Curva de dificultad en "flow"
- **Dificultad dinámica suave:** si el jugador falla 3 veces seguidas, baja un poco la
  densidad de obstáculos (sin decírselo); si gana fácil, sube el reto.
- **Cada barrio enseña una mecánica nueva** y el siguiente la combina con las anteriores.
- **Reto diario con semilla única:** el mismo mapa para todos ese día, para comparar
  puntajes en el ranking local o compartir el resultado como texto
  ("RutaMax #42 — ⭐⭐⭐ 2:14 — Eficiencia 94%").
- **Metas escalonadas siempre visibles:** una de corto plazo (este turno), una de
  mediano (desbloquear el próximo barrio) y una de largo (llegar a Monserrate).

### 11.4 Ética del diseño (obligatorio)
- **El progreso viene de la habilidad**, no de las horas jugadas ni de pagar.
- **Sin loot boxes, sin pay-to-win, sin compras reales** y sin temporizadores de
  espera para volver a jugar.
- **Rachas que perdonan:** perder un día no borra la racha, solo la congela
  ("comodín de descanso").
- **Sin FOMO castigador:** las misiones diarias dan bonus, pero perderlas no quita nada.
- **Pausas sanas:** después de unos 30 minutos seguidos, el Copiloto sugiere con
  humor: "Veci, llevamos rato rodando. ¿Un tintico y seguimos?" (desactivable).
- **Perder también es divertido:** los game over muestran frases chistosas y un dato
  útil para mejorar, nunca un castigo.

## 12. Estructura del juego (pantallas y estados)
Implementa una máquina de estados clara:

```
BOOT → SPLASH → MENÚ PRINCIPAL → SELECCIÓN DE BARRIO/TURNO → COUNTDOWN
     → JUGANDO ⇄ PAUSA → FIN DE TURNO → RESUMEN → (GARAJE | MENÚ | REINTENTAR)
```

- **Splash / Start:** logo propio del juego animado sobre Bogotá con los cerros de
  fondo, "Toca para empezar" y la carga de assets con barra de progreso.
- **Menú principal:** Jugar, Garaje/Tienda, Misiones, Ranking, Logros, Cómo jugar,
  Ajustes, Créditos. Fondo animado con la ciudad y lluvia suave.
- **Selección de barrio y turno:** tarjetas por barrio con dificultad, clima del día
  y si hay pico y placa o ciclovía; aquí se elige el vehículo.
- **Tutorial interactivo (primera vez):** 30 s guiados por el Copiloto IA: moverse,
  Nitro, seguir una ruta y entregar.
- **Countdown:** "3, 2, 1… ¡Arranque, veci!"
- **HUD en juego:** tiempo restante, pedidos pendientes, balance de Plus, barra de
  Nitro, minimapa, panel del Copiloto IA, clima y botón de pausa.
- **Pausa:** Continuar, Reiniciar turno, Ajustes, Salir al menú. Se pausa sola si la
  pestaña pierde el foco.
- **Fin de turno / Game Over:** se dispara por tiempo agotado, paquetes destruidos o
  batería en cero. Muestra la causa: "Se le acabó la batería subiendo a La Calera, parce."
- **Resumen:** entregas completadas, estrellas, Plus ganados, combo máximo, eficiencia
  frente a la IA, replay de la ruta, nuevos logros y botones de Reintentar, Garaje y Menú.
- **Garaje / Tienda:** comprar y mejorar vehículos, skins y power-ups.
- **Ajustes:** música, efectos, vibración, dificultad (Tranqui / Normal / Hora pico),
  calidad gráfica, idioma y controles.
- **Créditos.**

---

## 13. Renderizado y animaciones (calidad AAA en el navegador)

### 13.1 Pipeline de render
- Canvas 2D con soporte para `devicePixelRatio` (nítido en Retina) y escalado
  responsive sin distorsión.
- **Render por capas:**
  1. **Mapa estático** (calles, andenes, manzanas) pre-renderizado UNA vez en un
     `OffscreenCanvas` por chunk y reutilizado.
  2. **Capa dinámica:** vehículos, NPC, monedas, obstáculos, partículas.
  3. **Iluminación y clima:** noche, faros, lluvia, neblina.
  4. **UI/HUD** en DOM o en un canvas aparte, encima de todo.
- Game loop con **timestep fijo** (update a 60 Hz) y **render interpolado** con
  `requestAnimationFrame`, para que el movimiento sea igual de suave a 60/120/144 Hz.
- **Vista 2.5D:** edificios "extruidos" (techo desplazado según la cámara para dar
  sensación de altura y paralaje), sombras proyectadas suaves y los cerros
  orientales al fondo con paralaje lento.
- **Arte 100% procedural** dibujado en código (formas vectoriales, sin imágenes
  externas) y cacheado como sprites en `OffscreenCanvas`. Mismo grosor de contorno y
  la misma paleta en todo el juego.

### 13.2 Cámara
- Seguimiento con suavizado (lerp) y **look-ahead**: la cámara se adelanta hacia
  donde vas.
- **Zoom dinámico:** se aleja con el Tinto Nitro y se acerca al llegar a una entrega.
- **Screen shake basado en "trauma"** (decae con el tiempo, intensidad según el
  impacto): fuerte en los huecos, suave en las frenadas.
- **Hit-stop** de 50–80 ms al caer en un hueco grande.

### 13.3 Animaciones del vehículo
- Rotación suavizada hacia la dirección de movimiento.
- **Lean** de la carrocería en las curvas y **rebote de suspensión** (bob) al andar.
- **Squash & stretch** al caer en un hueco y al arrancar con Nitro.
- Ruedas que giran según la velocidad; **marcas de derrape** (decals) que se desvanecen.
- **Nitro:** estela de energía amarilla, motion lines y leve desenfoque radial.
- Luces de freno al desacelerar y **faros con cono de luz** en el modo noche.
- Salpicaduras de agua en los charcos y polvo en las calles destapadas.
- El paquete en la parrilla se balancea; si está dañado tiembla y muestra una grieta.
- Animación propia por vehículo: pedaleo (bici), vibración (moto), hélices y sombra (dron).

### 13.4 Animaciones de entrega y recompensas
- El paquete sale en **arco** (curva bezier + easeOutQuad) hasta la puerta y aterriza
  con un pequeño rebote.
- Confeti amarillo/azul, estrellas que aparecen una por una (easeOutBack) y un globo de
  diálogo del cliente con su frase rola.
- **Monedas Plus:** giran (scaleX con seno), tienen brillo pulsante y, al recogerlas,
  vuelan en curva hacia el contador del HUD, que "late" al recibirlas.
- El contador de combos crece con elastic easing y cambia de color según el nivel.
- Pin de destino que rebota y anillo pulsante en el punto de entrega.

### 13.5 Mundo vivo (Bogotá animada)
- Peatones con ciclo de caminata, vendedores ambulantes, bicitaxis y perritos que
  mueven la cola.
- Buses articulados de TransMilenio que se doblan en las curvas y buses del SITP.
- Tráfico NPC que frena y acelera; semáforos animados.
- Árboles que se mecen con el viento y palomas que vuelan en la Plaza de Bolívar.
- **Huecos:** borde irregular, charco con reflejo cuando llueve y pulso rojo de
  advertencia cuando la IA los reporta.
- **Clima con transiciones graduales:** el cielo se oscurece antes del aguacero, gotas
  en diagonal con ondas en los charcos, granizo que rebota, neblina con ruido animado
  y relámpagos ocasionales.
- **Ciclo día/noche:** overlay de color según la hora, luces de las ventanas que se
  encienden y Monserrate iluminado de noche.

### 13.6 Animaciones del Copiloto IA
- Ruta sugerida como línea con **dash animado que fluye** (`lineDashOffset`), con un
  color distinto por perfil.
- "Recalculando…": la ruta vieja se desvanece y la nueva se dibuja progresivamente.
- Ícono del copiloto con animación de "pensando" y texto con efecto **typewriter**.
- Mapa de calor con gradientes suaves que "respiran" (pulso lento).
- Reportes de la comunidad que aparecen en el mapa con un pop y se desvanecen.

### 13.7 Animaciones de UI y pantallas
- Sistema de tweens propio con funciones de easing: easeOutCubic, easeOutBack,
  easeInOutSine, elastic, bounce.
- **Transiciones entre escenas:** fade, slide y un "wipe" con forma de caja que se abre.
- **Splash:** el logo se arma por partes, la caja se cierra con la cinta amarilla y
  la ciudad aparece detrás.
- **Menú:** botones con hover (scale 1.05 + sombra) y press (scale 0.95), tarjetas que
  entran escalonadas (stagger de 60 ms).
- **Countdown:** números que golpean la pantalla (scale + shake).
- **Resumen:** los números cuentan hacia arriba (count-up), las estrellas se llenan una
  por una, la barra de XP se llena y el replay se dibuja en el minimapa.
- **Game Over:** cámara lenta de 0.5 s, desaturación de color y la causa entrando con
  efecto de rebote.

### 13.8 Rendimiento
- **Object pooling** para partículas, monedas, NPC y decals (cero allocations dentro
  del loop).
- **Culling:** no dibujar nada fuera de la cámara.
- Límite de partículas según la calidad elegida (Baja / Media / Alta) y auto-ajuste si
  los FPS bajan de 50.
- Respetar `prefers-reduced-motion` (menos shake y partículas).
- **Debug overlay con F3:** FPS, número de entidades, draw calls y grafo de rutas de la IA.

---

## 14. Requisitos técnicos
- **Un solo archivo HTML** con JS y CSS embebidos. Sin librerías, sin imágenes
  externas, sin fuentes externas: todo el arte y el sonido se generan por código.
- **No uses la cámara ni el micrófono** (getUserMedia): agregan permisos, fricción y
  problemas de privacidad, y no mejoran este tipo de juego.

### 14.1 Controles (pocos y claros)
Pensados para un PC portátil con teclado y trackpad, que es el escenario principal.

| Acción | Teclado (principal) | Trackpad / mouse | Táctil (mobile) |
|---|---|---|---|
| Moverse | Flechas o WASD | — | Joystick virtual (lado izquierdo) |
| Tinto Nitro | Espacio | — | Botón grande (lado derecho) |
| Elegir ruta de la IA | Teclas 1 / 2 / 3 | Clic en la tarjeta de la ruta | Tap en la tarjeta |
| Reportar obstáculo | E | Clic en el botón "Reportar" | Tap en el botón |
| Pausa | P o Esc | Clic en ⏸ | Tap en ⏸ |
| Reintentar al instante | R | Clic en "Otra vez" | Tap en "Otra vez" |
| Menús | Flechas + Enter | Clic | Tap |

- Conducir es **solo con el teclado** (o el joystick en mobile): el trackpad no sirve
  para un control fino y continuo, así que se usa solo para menús y para elegir rutas.
- Muestra los controles en el tutorial y como chips discretos en el HUD durante los
  primeros turnos.
- Detecta automáticamente si el dispositivo es táctil y muestra el joystick solo en ese caso.
- Soporte opcional de gamepad (Gamepad API) si hay uno conectado, sin que sea obligatorio.

### 14.2 Ganar y perder (condiciones medibles)
- **Turno:** dura 180 s. Cada barrio tiene una meta de entregas (Chapinero: 5).
  - **Ganas el turno** si cumples la meta antes de que el reloj llegue a 0.
  - **Pierdes el turno** si: el reloj llega a 0 sin cumplir la meta, la integridad de
    los paquetes cae a 0% o la batería llega a 0%.
- **Pedido:** tiempo límite = ETA de la ruta óptima de la IA × 1.5.
  - A tiempo: Plus completos. Tarde: la mitad de los Plus y una estrella menos del cliente.
  - Frágil con más del 50% de daño: el cliente lo rechaza y no cuenta para la meta.
- **Estrellas del turno:**
  - ⭐ Cumplir la meta.
  - ⭐⭐ Meta + daño total menor al 20%.
  - ⭐⭐⭐ Meta + daño menor al 20% + eficiencia frente a la IA de al menos 90%.
- **Eficiencia** = distancia de la ruta óptima / distancia que recorriste × 100.
- Todos estos valores van en el objeto `CONFIG` para poder ajustarlos.
- **Mapa procedural** en grilla con semilla, respetando la nomenclatura bogotana.
- **Arquitectura modular:**
  `StateMachine`, `Scenes` (Splash, Menu, Select, Play, Pause, End, Summary, Garage,
  Settings, Credits), `MapGraph`, `RoutePlanner` (A* + VRP), `AICopilot`, `Obstacles`,
  `Weather`, `Economy`, `Audio`, `UI`, `SaveSystem`, `Renderer` (capas + cache),
  `Camera`, `TweenSystem`, `ParticleSystem`, `Lighting`.
- 60 FPS estables; progreso guardado en `localStorage` envuelto en `try/catch`
  (si falla, el juego sigue funcionando en memoria).
- Responsive para desktop y mobile.
- Código comentado en español, con constantes de balance (velocidades, tiempos,
  precios) agrupadas en un objeto `CONFIG` al inicio para ajustarlas fácilmente.

---

## 15. Entrega por fases
| Fase | Alcance |
|---|---|
| **1** | Máquina de estados con Splash, Menú, Jugar, Pausa, Fin y Resumen; un barrio (Chapinero), un vehículo (moto), pedidos seguidos hasta la meta de 5 con el reloj de 180 s, condiciones de ganar y perder, huecos, monedas Plus, puntaje, reinicio rápido con R, controles completos de teclado, screen shake, partículas, easing y animaciones base del vehículo. |
| **2** | Copiloto IA con A*, 3 perfiles, re-ruteo animado, explicabilidad y reportes de la comunidad. |
| **3** | Multi-entrega (VRP), power-ups bogotanos, economía, Garaje, clima y ciclo día/noche. |
| **4** | Más barrios, pico y placa, ciclovía, TransMilenio, minijuego de dirección errada, Pedido Jefe Monserrate, replay, ranking, logros y pulido visual y sonoro. |

**Arranca con la Fase 1 completa y jugable.** Al final de cada fase:
1. Entrega el archivo HTML completo (no fragmentos).
2. Resume las decisiones de diseño que tomaste.
3. Lista qué sigue en la próxima fase.

---

## 16. Criterios de aceptación
- [ ] Puedo ir de Splash → Menú → Jugar → Fin → Resumen → Menú sin errores en consola.
- [ ] Puedo pausar y reanudar en cualquier momento.
- [ ] Las direcciones usan el formato bogotano real y el minijuego de dirección errada
      se entiende.
- [ ] La IA siempre explica por qué sugiere cada ruta, con tono rolo.
- [ ] Se nota la diferencia entre seguir la IA y no seguirla.
- [ ] Todo objeto en movimiento tiene animación (nada aparece ni desaparece de golpe).
- [ ] Se mantienen 60 FPS estables en un portátil promedio con la calidad en Alta.
- [ ] Al jugar se siente el "juice": cada acción tiene feedback visual y sonoro.
- [ ] Después de perder, quiero darle "una más" porque sé exactamente qué mejorar.
- [ ] Los últimos 10 s de un pedido generan tensión real (visual, sonora y de ritmo).
- [ ] Reintentar toma menos de 1 s.
- [ ] Todo el juego se puede jugar solo con el teclado de un portátil, sin mouse.
- [ ] El archivo funciona sin conexión a internet (cero requests externos).
- [ ] Ninguna mecánica castiga por no jugar ni empuja a pagar.
- [ ] Se siente rápido, jugoso, chistoso y muy bogotano.
