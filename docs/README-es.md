🇬🇧 [English](../README.md) | 🇩🇪 [Deutsch](README-de.md) | 🇪🇸 [Español](README-es.md)

# Cosmic Web Explorer

[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.18915566-blue.svg)](https://doi.org/10.5281/zenodo.18915566) y [![arXiv](https://img.shields.io/badge/arXiv-2603.18145-b31b1b.svg)](https://arxiv.org/abs/2603.18145)

[** Demostración en vivo**](https://nicosmo.github.io/cosmic_web_explorer/)


<p align="center">
  <video src="https://github.com/user-attachments/assets/a6712709-7ea4-48f9-bd26-4f384046de06" autoplay loop muted playsinline width="40%"></video>
</p>



**Concepto inicial de [Simon Bouchard](https://github.com/SBouchard01) y [Nico Schuster](https://orcid.org/0000-0001-5620-8554) | Desarrollo de [Nico Schuster](https://orcid.org/0000-0001-5620-8554)**


Una simulación interactiva y en tiempo real de la estructura a gran escala (LSS) del Universo, que se ejecuta íntegramente en tu navegador.

Este proyecto visualiza la evolución no lineal de una distribución inicial de trazadores de materia. Observa cómo las fluctuaciones primordiales colapsan bajo la influencia de la gravedad y la expansión cósmica para formar una intrincada red de vacíos, filamentos y halos cósmicos.

La evolución se calcula combinando la teoría de perturbaciones lagrangianas de segundo orden (2LPT) analítica con un esquema local de gravedad cuasi-N-cuerpos (activado en el modo «Reproducir»). La simulación aprovecha WebGL para un renderizado de alto rendimiento y WebGPU (con una alternativa en CPU) para los cálculos físicos en paralelo.


> **Nota sobre la precisión física**
> Esta es una herramienta pedagógica y de visualización. Para mantener un rendimiento fluido e interactivo directamente en el navegador, el Cosmic Web Explorer se entiende mejor como un «Universo 2.5D»: una rebanada fina de un universo 3D proyectada sobre un plano 2D. Hacemos varios compromisos estrictos en cuanto a la precisión física:
> - *Interacciones y fuerzas:* Aunque la expansión de fondo $H(z)$, el factor de crecimiento $D_1(z)$ y las funciones de transferencia se derivan rigurosamente de la física 3D, las interacciones entre partículas ocurren en un plano 2D. En consecuencia, el uso de una fuerza gravitatoria 2D $1/r$ sacrifica la formación realista de halos 3D densos en favor de una red de filamentos cósmicos acelerada y altamente visual.
> - *Límites de resolución:* Los recuentos bajos de trazadores (20k - 200k) pueden causar artefactos estructurales. Además, las fuerzas gravitatorias locales se truncan artificialmente para mantener una tasa de fotogramas interactiva.
> - *Modelos fenomenológicos:* No hay hidrodinámica real del gas. La dinámica sin colisiones se aproxima mediante un modelo de «Adhesión» para evitar que los cúmulos exploten visualmente, y se implementa un modelo fenomenológico para impedir que los trazadores colapsen en masas puntuales.
> - *Espacio de corrimiento al rojo:* Se ofrece para intuición visual y no es completamente exacto. El campo de velocidades en la línea de visión se modela de forma aproximada para resaltar los patrones de distorsión, por lo que la amplitud de RSD mostrada debe interpretarse cualitativamente, no como una predicción observacional exacta.

---


## Galería

| Modo BAO esculpido | Vista térmica del gas |
| :---: | :---: |
| <a href="../Examples/BAO_example.png"><img src="../Examples/BAO_example.png" width="350"></a> | <a href="../Examples/Thermal_example.png"><img src="../Examples/Thermal_example.png" width="350"></a> |
| *Evolución a partir de condiciones iniciales (ICs) BAO.* | *Imitando la temperatura local.* |


| LSS realista | Identificación de vacíos |
| :---: | :---: |
| <a href="../Examples/LSS_example.png"><img src="../Examples/LSS_example.png" width="350"></a> | <a href="../Examples/Voids_example.png"><img src="../Examples/Voids_example.png" width="350"></a> |
| *Evolución a partir de ICs de Eisenstein & Hu P(k).* | *Identificación de vacíos en tiempo real.* |

| Movimiento de los trazadores | Comparación en pantalla dividida |
| :---: | :---: |
| <a href="../Examples/Trails_example.png"><img src="../Examples/Trails_example.png" width="350"></a> | <a href="../Examples/Comparison_example.png"><img src="../Examples/Comparison_example.png" width="350"></a> |
| *Representa el movimiento pasado de los trazadores.* | *Comparación cosmológica entre dos paneles.* |


---

## Contribuir y comentarios

¡Las contribuciones, sugerencias de funcionalidades e informes de errores son muy bienvenidos!

* **Issues:** Si tienes una idea, quieres discutir una aproximación física o encontraste un error, abre un issue en GitHub.
* **Pull Requests:** Si deseas contribuir con código, puedes hacer un fork del repositorio y enviar un Pull Request.

---

## Construido con
* **Interfaz e interactividad:** React 18, TailwindCSS y Babel (Standalone). Diseñado con una **arquitectura sin compilación** (*zero-build*): se ejecuta íntegramente en el navegador sin Node.js ni Webpack.
* **Gráficos y renderizado:** WebGL personalizado (shaders) y la API HTML5 Canvas.
* **Cómputo físico:** WebGPU (compute shaders) con una alternativa síncrona de hash espacial en CPU.
* **Geometría computacional:** d3-delaunay (ejecutado fuera del hilo principal, en Web Workers en segundo plano).
* **Codificación de vídeo:** API WebCodecs (H.264 por hardware) y ffmpeg.wasm.

---

## Funcionalidades

### Simulación interactiva y cosmología
* **Evolución en tiempo real:** Observa cómo se forman las estructuras cósmicas desde el corrimiento al rojo $z=10$ (universo temprano) hasta $z=0$ (hoy), de forma nativa en el navegador.
* **Condiciones iniciales configurables:** Elige entre condiciones iniciales (ICs) realistas e ICs mejoradas con BAO (detalles más abajo, en la sección de ciencia). También puedes fijar el número de trazadores (entre 20.000 y 200.000) y ajustar el campo de visión (500-3000 Mpc).
* **Controles cosmológicos:** Ajusta los parámetros cosmológicos para observar de inmediato su impacto en el crecimiento de estructuras. Soporta $\Lambda\text{CDM}$ plano/no plano estándar, $w\text{CDM}$ y energía oscura dinámica ($w_0w_a\text{CDM}$ mediante la parametrización CPL).
* **Control del tiempo:** Retrocede y avanza por la historia cósmica con el deslizador temporal (calcula al instante los desplazamientos 2LPT, sin gravedad local), o pulsa «Reproducir» para una evolución más precisa.
* **Comparación en pantalla dividida:** Ejecuta dos simulaciones independientes en paralelo. Fija parámetros cosmológicos distintos para el panel A y el panel B compartiendo exactamente la misma semilla aleatoria. El panel B se sincroniza con el corrimiento al rojo ($z$) del panel A para comparar visualmente la formación de estructuras en la misma etapa de la expansión cósmica.
* **Espacio de corrimiento al rojo:** Cambia entre espacio real y espacio de corrimiento al rojo para desplazar los trazadores a lo largo de una línea de visión seleccionable, debido al efecto Doppler de su propio movimiento, usando velocidades peculiares modeladas. La intensidad de las distorsiones del espacio de corrimiento al rojo (RSD) es ajustable para comparar visualmente el agrupamiento anisótropo.
* **Mejora ALPT (opcional):** Combina el desplazamiento 2LPT con un término de colapso esférico en escalas pequeñas, siguiendo la idea de la LPT aumentada de Kitaura & Heß (2013). Un radio de suavizado $r_s$ controla la transición 2LPT → colapso esférico. La implementación usa un análogo 2D documentado del modelo 3D publicado (véase la sección de ciencia). Opcionalmente puede activarse además un pequeño paso extra a lo largo del gradiente local de densidad, que empuja los trazadores hacia los máximos locales de densidad (véanse las secciones 2.4 y 2.5).

### Modos de visualización
* **Trazadores y estelas:** Visualiza la distribución estándar de trazadores de masa y activa los modos **Historial** (trayectorias pasadas) o **Vector** (velocidad actual exagerada) para visualizar los flujos globales.
* **Vista térmica (gas):** Aproxima el calentamiento del gas en estructuras colapsadas mediante gradientes radiales aditivos. Los colores se corresponden con la densidad local ($\text{Azul} \to \text{Cian} \to \text{Blanco} \to \text{Dorado}$), mostrando el calentamiento del gas al caer en los cúmulos.
* **Paletas de color:** Un interruptor **Múltiples escalas de color** en *Vista y herramientas* sustituye el aspecto fijo de densidad y térmico por una elección de ocho paletas (Classic, Viridis, Inferno, Plasma, Magma, Turbo, Cividis, Cool–Warm) para los trazadores de densidad y para el gas térmico de forma independiente, cada una con una vista previa del gradiente en vivo. Cada paleta incluye además una variante **invertida** (p. ej. *Viridis (invertida)*): el mapa de densidad va de brillante a saturado sobre un fondo de papel claro, de modo que las galaxias se leen como tinta de color, y la vista térmica pasa de la mezcla aditiva (solo válida sobre negro) a la composición alfa ordinaria. Las capturas y grabaciones heredan el fondo claro y cambian la marca de agua a tinta oscura. Ten en cuenta que la inversión es un giro literal de la LUT, así que las paletas cuyo rango medio es brillante (Classic) conservan un contraste menor sobre el papel.
* **Superposición de condiciones iniciales:** Resalta las posiciones originales de la malla lagrangiana en $z=10$ para seguir los flujos globales desde su origen.
* **Superposición de imagen personalizada:** Carga una imagen externa (por ejemplo, datos de cartografiado de galaxias o imágenes de simulaciones de referencia) para compararla directamente con la red cósmica en evolución. Incluye deslizadores dedicados para ajustar con precisión la opacidad, la escala y la posición X/Y para alinearla.

### Herramientas de análisis
* **Teselación de Voronoi:** Muestra las celdas de Voronoi de los trazadores, resaltando las regiones subdensas.
* **Identificación de vacíos en tiempo real:** Encuentra vacíos cósmicos usando un Web Worker en segundo plano que ejecuta un algoritmo Voronoi-Watershed (similar a [VIDE](https://ui.adsabs.harvard.edu/abs/2015A%26C.....9....1S/abstract)). Filtra las celdas de vacío con un «Corte de color», fusiónalas con un «Radio mínimo» y muestra centros geométricos o de mínima densidad.
* **Perfiles de densidad de vacíos:** Calcula y representa los perfiles radiales de densidad ($\rho(r)/\bar{\rho}$) de los vacíos identificados, usando apilado por mediana o ponderado por volumen.
* **Espectro de potencias $P(k)$:** En el modo de IC realistas, extrae el espectro de potencias 2D del campo de densidad evolucionado. Incluye evaluación exacta en mallas que no son potencias de 2 (FFT de Bluestein) e *interlacing* ([Sefusatti et al. 2016](https://ui.adsabs.harvard.edu/abs/2016MNRAS.460.3624S/abstract)).
* **Correlación de 2 puntos:** En el modo BAO, evalúa la correlación cruzada $\xi(r)$ de los trazadores alrededor de las semillas BAO conocidas.
* **Distribución de densidad:** Un histograma normalizado en tiempo real del contraste de densidad de los trazadores ($\delta = \rho/\bar{\rho} - 1$), que sigue el paso de fluctuaciones gaussianas lineales a una distribución no lineal asimétrica.
* **Clasificación de la red cósmica:** Un clasificador T-web 2D (hessiano de la densidad suavizada o del potencial gravitatorio) pinta cada píxel como nudo, filamento o vacío, con una leyenda de color en el propio lienzo. El campo, la escala de suavizado y el umbral se ajustan en vivo.
* **Regla:** Mide distancias comóviles (Mpc, suponiendo $h = 0.674$) directamente sobre la red. *(Interacción: clic para iniciar, clic para terminar, doble clic cerca del inicio para borrar).*

### Interactividad y exportación de alta calidad
* **Modo escultura:** En $z=0$, interactúa directamente con el motor físico. *(Interacción: haz clic y arrastra para crear pozos gravitatorios manuales y atraer físicamente los filamentos).*
* **Grabación de vídeo de alta calidad:** Una tubería de renderizado determinista exporta vídeos perfectos fotograma a fotograma, independientemente de los tirones del navegador. Graba exactamente lo que ves o fuerza 720p/1080p. Utiliza WebCodecs por hardware (H.264), ffmpeg.wasm o el ensamblado WebM en JavaScript puro.
* **Exportación de capturas:** Descarga PNG de alta resolución de la visualización actual, con superposiciones HUD físicas que detallan la edad cósmica y el corrimiento al rojo.
* **Exportación de datos (CSV):** Exporta las coordenadas comóviles exactas (Mpc/h) de todos los trazadores a un archivo CSV (soporta por completo la exportación dual simultánea en modo de pantalla dividida).

### Impronta de texto: escribe en la red cósmica

En el control **Semilla** de la barra lateral, marca **«Añadir tu propio texto»**, escribe una palabra (hasta 16 caracteres) y pulsa **Aplicar**. El texto será invisible en $z = 10$ y emergerá gradualmente como filamentos densos a medida que el universo evoluciona hacia $z \approx 0$.

La impronta funciona secuestrando las correcciones de la teoría de perturbaciones lagrangianas de segundo orden (2LPT). Las condiciones iniciales primarias (1LPT) permanecen completamente puras y físicas. Como la corrección 2LPT escala cuadráticamente con el factor de crecimiento ($D_1(z)^2$), el desplazamiento del texto es prácticamente nulo en $z = 10$ y, matemáticamente, fuerza a las partículas a converger sobre los trazos de las letras exactamente cuando $D_1(z) \rightarrow 1$. La red cósmica circundante no resulta afectada. La funcionalidad es **opcional y está desactivada por defecto**; cuando no está marcada, la simulación es idéntica a la ejecución estándar.

Para una descripción detallada de cómo funciona, véase [IMPRINT.md](../IMPRINT.md).

---


## Primeros pasos

Como este proyecto utiliza una **arquitectura sin compilación**, que depende por completo de tecnologías nativas del navegador (HTML5, WebGL, WebGPU, Web Workers) y de bibliotecas servidas por CDN, no se requiere `npm install` ni una tubería de compilación compleja.

### Requisitos
* Un navegador web moderno (Chrome, Edge, Firefox, Safari).
* **Recomendado:** un navegador con WebGPU habilitado para el cómputo físico en paralelo. Si WebGPU no está disponible, la simulación recurrirá sin problemas a una implementación síncrona de hash espacial en CPU.

### Ejecución local
Para evitar las restricciones de seguridad de origen cruzado (CORS) del navegador al cargar Web Workers y módulos locales, los archivos deben servirse mediante un servidor HTTP local:

1.  Abre tu terminal y entra en la carpeta del proyecto.
2.  Inicia un servidor local (por ejemplo, con Python): `python3 -m http.server 8080` (o tu servidor local preferido).
3.  Abre `http://localhost:8080` en tu navegador.

---


## Estructura del proyecto

```text
cosmic_web_explorer/
├── index.html              # HTML principal que contiene la interfaz React
├── styles.css              # Estilos de la aplicación
├── .github/
│   └── workflows/
│       └── smoke.yml         # Prueba de humo de arranque en CI
├── docs/                   # Traducciones
├── Examples/               # Vídeos de demostración e imágenes de la galería
├── tests/
│   └── smoke.spec.js         # Cobertura de humo de arranque/montaje con Playwright
├── src/
│   ├── constants.js          # Parámetros cosmológicos (Planck 2018)
│   ├── cosmology.js          # LUTs de historia de expansión y factor de crecimiento
│   ├── transfer-function.js  # Funciones de transferencia P(k) de Eisenstein & Hu
│   ├── pert2lpt.js           # Desplazamiento 2LPT mediante FFT 2D
│   ├── pertalpt.js           # LPT aumentada: mezcla de colapso 2D + paso de gradiente
│   ├── sim-physics.js        # Integración Leapfrog KDK y modelo de adhesión
│   ├── redshift-space.js     # Desplazamiento LOS en espacio de redshift y lógica de velocidad híbrida
│   ├── analysis-keys.js      # Ayudantes compartidos de clave/firma de caché para espacios de análisis
│   ├── panel-render-loop.js  # Tubería extraída de física y RSD por panel
│   ├── startup-guard.js      # Comprobaciones previas de arranque y respaldo global de errores fatales
│   ├── bao-forces.js         # Lógica de inicialización BAO esculpida
│   ├── gpu-gravity.js        # Compute shader N-cuerpos en WebGPU
│   ├── gpu-correlation.js    # Compute shader de conteo de pares en WebGPU
│   ├── web-classify.js       # Clasificación T-web 2D de la red cósmica (nudo/filamento/vacío)
│   ├── sim-renderer.js       # Lógica de dibujo Canvas2D para superposiciones y gráficos
│   ├── webgl-utils.js        # Renderizado WebGL de trazadores y gas
│   ├── color-luts.js         # Mapas de color para densidad/temperatura
│   ├── recorder.js           # Codificador de vídeo con múltiples tuberías
│   ├── ui-components.js      # Elementos reutilizables de la interfaz React (tooltips)
│   └── icons.js              # Iconos SVG
└── workers/
    └── void-worker.js        # Búsqueda de vacíos en segundo plano (Delaunay/Watershed)
```

---

## La ciencia

### 1. Fondo cosmológico

El núcleo de la visualización se apoya en la historia de expansión de fondo de un universo Friedmann-Lemaître-Robertson-Walker (FLRW) para dictar el paso del tiempo y la tasa de formación de estructuras.

**Historia de expansión:**
La tasa de expansión de Hubble adimensional, $E(z) \equiv H(z)/H_0$, gobierna la fricción cósmica y se calcula como:

$$
E(z) = \sqrt{\Omega_m (1+z)^3 + \Omega_k (1+z)^2 + \Omega_{DE}(z)}
$$

donde $\Omega_k = 1 - \Omega_m - \Omega_\Lambda$. Para soportar modelos de energía oscura dinámica ($w_0w_a \text{CDM}$), implementamos la parametrización CPL:

$$
\Omega_{DE}(z) = \Omega_\Lambda (1+z)^{3(1+w_0+w_a)} \exp\left(-\frac{3 w_a z}{1+z}\right)
$$

**Integración del tiempo y del crecimiento:**
Asignar el corrimiento al rojo ($z$) a los fotogramas de la simulación requiere calcular el tiempo cósmico $t(z)$ y el factor de crecimiento lineal $D_1(z)$, que escala los desplazamientos de las partículas. Para mantener tasas de fotogramas interactivas sin resolver estas integrales al vuelo, el motor precalcula una tabla de consulta (LUT) de 10.000 pasos para un muestreo $O(1)$:

$$
t(z) = \frac{1}{H_0} \int_z^\infty \frac{dz'}{(1+z') E(z')}
$$
$$
D_1(z) \propto E(z) \int_z^\infty \frac{1+z'}{E(z')^3} dz'
$$


### 2. Condiciones iniciales: dos modos

La visualización ofrece dos modos distintos para generar el campo de densidad en $z=10$, atendiendo tanto a la pedagogía visual como al rigor estadístico.

#### 2.1 Modo riguroso $P(k)$
Para una representación matemáticamente precisa de las fluctuaciones primordiales, utilizamos la función de transferencia de [Eisenstein & Hu (1998)](https://ui.adsabs.harvard.edu/abs/1998ApJ...496..605E/abstract).

* **Proyección de 3D a 2D:** Como la visualización opera sobre un lienzo 2D, el espectro de potencias teórico 3D $P_{3D}(k)$ se proyecta sobre una rebanada 2D mediante una integral de Abel, para garantizar propiedades de agrupamiento estadísticamente correctas:

$$
P_{2D}(k_{\perp}) = \frac{1}{2\pi} \int_{-\infty}^{\infty} P_{3D}\left(\sqrt{k_{\perp}^2 + k_z^2}\right) dk_z
$$

* **Campo aleatorio gaussiano:** El $P_{2D}(k)$ proyectado se asigna a una malla en el espacio de Fourier, se siembra con una distribución aleatoria gaussiana de Box-Muller y se transforma inversamente (FFT) para producir el campo de desplazamiento de Zel'dovich 1LPT inicial $\mathbf{\Psi}^{(1)}$.

#### 2.2 Modo BAO fenomenológico «esculpido»
Como la señal BAO real es estadísticamente débil (contraste de ~1%) y difícil de aislar a simple vista en volúmenes pequeños y con baja resolución, ofrecemos un modo esculpido diseñado para resaltar explícitamente la escala acústica.

Construimos un campo de desplazamiento $\mathbf{\Psi}(\mathbf{q})$ superponiendo cáscaras potenciales gaussianas radiales alrededor de un conjunto de centros aleatorizados $\{ \mathbf{x}_c \}$. Esto crea sobredensidades claras en los centros y cáscaras esféricas a la escala BAO. Las condiciones iniciales de fondo se siguen generando a partir del espectro de potencias proyectado.

#### 2.3 Teoría de perturbaciones lagrangianas de segundo orden (2LPT)
Para capturar rasgos no gaussianos, en particular el afilado de los filamentos y el redondeo de los vacíos, antes del paso de relajación de N-cuerpos implementamos 2LPT. La posición euleriana $\mathbf{x}$ de una partícula en el tiempo $t$ viene dada por:
$$\mathbf{x}(t) = \mathbf{q} + D_1(t)\mathbf{\Psi}^{(1)}(\mathbf{q}) + D_2(t)\mathbf{\Psi}^{(2)}(\mathbf{q})$$

Suponemos la aproximación estándar válida para cosmologías del tipo $\Lambda\text{CDM}$:
$$D_2(t) \approx -\frac{3}{7} D_1(t)^2$$

**Cálculo mediante FFT:** El código calcula el potencial de desplazamiento de segundo orden $\phi_2$ resolviendo la ecuación de Poisson en una malla mediante transformadas rápidas de Fourier:

$$
\nabla^2 \phi_2 = \phi_{1,xx} \phi_{1,yy} - (\phi_{1,xy})^2
$$

El término fuente es el determinante del hessiano del potencial de primer orden. El desplazamiento 2LPT es entonces el gradiente de este potencial: $\mathbf{\Psi}^{(2)} = - \nabla \phi^{(2)}$.

#### 2.4 LPT aumentada (opcional)

El interruptor **ALPT** combina el desplazamiento 2LPT con un desplazamiento de colapso esférico (SC) en escalas pequeñas, siguiendo a Kitaura & Heß (2013):

$$
\mathbf{\Psi}_{\rm ALPT}(\mathbf{q}) = K(\mathbf{q},r_s)\,\mathbf{\Psi}_{\rm 2LPT}(\mathbf{q}) + \left[1-K(\mathbf{q},r_s)\right]\mathbf{\Psi}_{\rm SC}(\mathbf{q}),
\qquad K(\mathbf{q},r_s) = \exp\!\left(-\frac{|\mathbf{q}|^2}{2r_s^2}\right)
$$

La implementación se aparta intencionadamente del modelo 3D publicado en tres aspectos documentados:

* **Análogo de colapso 2D.** El campo de densidad de la visualización es una proyección 2D, por lo que el estiramiento SC usa la continuación dimensional a $d=2$
  $$\psi_{\rm SC,2D} = 2\left(\sqrt{1-\delta}-1\right), \qquad \psi_{\rm SC} \ge -2,$$
  en lugar de la ecuación (6) 3D de Kitaura & Heß, $\psi_{\rm SC,3D} = 3(\sqrt{1-2\delta/3}-1)$ con $\psi \ge -3$. Ambas comparten el límite lineal $\psi \approx -\delta$; la forma 2D es un análogo derivado, **no** una fórmula publicada.
* **Reconstrucción irrotacional.** El modelo SC especifica solo la divergencia $\psi_{\rm SC} = \nabla\cdot\mathbf{\Psi}_{\rm SC}$. Mezclamos las divergencias en el espacio de Fourier, resolvemos una ecuación de Poisson y tomamos el gradiente, de modo que el desplazamiento SC reconstruido es libre de rotacional. Esta es una elección estándar, pero es una suposición que el artículo no declara.
* **Dependencia temporal.** La ALPT publicada es un modelo de un solo paso en $z=0$. Aquí los campos LPT crecen con $D(t)$, por lo que el término SC se evalúa en el factor de crecimiento actual. Se construye una tabla de 13 muestras en $D$ una vez por cada cambio de parámetros y se interpola por fotograma; se elimina el modo $k=0$ para que la caja periódica no se contraiga globalmente.

$r_s$ se expone en $h^{-1}\mathrm{Mpc}$. El óptimo de 4–5 $h^{-1}\mathrm{Mpc}$ del artículo se refiere a densidades 3D y **no está calibrado** para este campo 2D proyectado: trátalo como una escala visual. Activar ALPT desactiva la inversión de la impronta de texto, que supone el desplazamiento base 2LPT puro.

#### 2.5 Paso local por gradiente (opcional)

Dentro del bloque ALPT hay un segundo interruptor independiente («Paso por gradiente local»). Cuando está activo, cada trazador recibe un desplazamiento extra a lo largo del gradiente **local** de densidad, lo que afila la estructura de pequeña escala más allá de lo que el término de colapso en el espacio de Fourier puede expresar:

$$
\mathbf{\Psi}_{\rm step}(\mathbf{q}) = \eta\,\hat{\nabla}\delta(\mathbf{q})\; s(\delta)\; \frac{|\nabla\delta|}{|\nabla\delta| + \epsilon},
\qquad
\mathbf{x}(t) = \mathbf{q} + D\mathbf{\Psi}^{(1)} + D^2\left(\mathbf{\Psi}^{(2)} + \mathbf{\Psi}_{\rm SC}\right) + D^3 \mathbf{\Psi}_{\rm step}
$$

Los términos se evalúan una vez por cada construcción de la tabla ALPT, sobre una malla de sobre-densidad suavizada periódicamente (2 pasadas), depositada a partir de las partículas evolucionadas con ALPT en la última muestra del factor de crecimiento. $\hat{\nabla}\delta$ es el gradiente unitario de densidad (diferencias centrales periódicas), $s(\delta)$ es una rampa *smoothstep* en sobre-densidad sobre $[-0.5, 0.5]$ que desactiva el paso en los vacíos y lo activa dentro de las estructuras, y el factor de fiabilidad $|\nabla\delta|/(|\nabla\delta|+\epsilon)$, con $\epsilon = 0.6\,\sigma_{|\nabla\delta|}$, amortigua el paso en mesetas y crestas planas donde el gradiente es mayormente ruido. La longitud del paso $\eta$ se fija con el deslizador en unidades del espaciado medio entre trazadores $\bar d$, y la rampa $D^3$ liga el efecto al crecimiento de estructuras más que al campo lineal. El paso es un desplazamiento puramente posicional: no añade velocidad, y por tanto no añade energía, y desactivar el interruptor reproduce exactamente el campo ALPT puro (verificado con precisión de float32).

Este paso está adaptado de la implementación de referencia del autor (`js/cosmic-web.js`, `buildLocalStep`, en dforero0896.github.io) y **no** es un término derivado de la teoría de perturbaciones. No es libre de rotacional, por lo que no conserva el campo de densidad lineal, y se aplica solo dentro de la tubería ALPT.



### 3. Dinámica: cuasi-N-cuerpos y adhesión

Para lograr tasas de fotogramas interactivas y estructuras estables en 2D, usamos un enfoque híbrido que modifica la dinámica estándar de N-cuerpos.

**El modelo de «Adhesión» (física de cúmulos)**
En las regiones de alta densidad, las trayectorias puramente LPT se cruzan y explotan (*shell-crossing*). En lugar de implementar un modelo viscoso de la ecuación de Burgers, computacionalmente costoso, usamos un modelo de adhesión fenomenológico. Rastreamos un «acumulador de cruce de cáscaras» $N_{cross}$ para cada partícula, que se incrementa cuando partículas cercanas tienen direcciones de desplazamiento antiparalelas:

$$
\mathbf{\Psi}_{1,i} \cdot \mathbf{\Psi}_{1,j} < 0 ,
$$

lo que indica flujos convergentes. El factor de crecimiento lineal efectivo para esa partícula se amortigua localmente:

$$
D_{\mathrm{eff}}^{(i)}(t) = \frac{D_1(t)}{1 + \alpha \cdot N_{cross}^{(i)}}
$$

Esto «congela» efectivamente la expansión del campo de desplazamiento en las regiones de alta densidad, imitando la formación de estructuras estables y virializadas. Para evitar un amortiguamiento espurio en regiones subdensas, los eventos de cruce solo se acumulan donde la densidad local supera $2 \cdot \bar{\rho}$.

**Gravedad local e integración**
Añadimos fuerzas de corto alcance mediante un método partícula-partícula sobre malla, con una ley de fuerza $1/r$ suavizada por Plummer (gravedad newtoniana 2D):

$$
\mathbf{F}_{ij} = G_{\mathrm{eff}} \cdot \frac{\mathbf{d}_{ij}}{|\mathbf{d}_{ij}|^2 + \varepsilon^2}
$$

La constante de acoplamiento $G_{\mathrm{eff}}$ escala heurísticamente con la tasa de crecimiento lineal adimensional $f(a) \approx \Omega_m(a)^{0.55}$ ([Linder 2005](https://ui.adsabs.harvard.edu/abs/2005PhRvD..72d3529L/abstract)) para asegurar que el agrupamiento se congele visualmente a medida que la energía oscura domina:
$$G_{\mathrm{eff}} = G_{\mathrm{base}} \cdot \Omega_m(a)^{0.55} \cdot \frac{\Delta t}{\Delta t_{\mathrm{ref}}} \cdot \frac{N_{\mathrm{ref}}}{N}$$

La integración se realiza con un esquema Leapfrog KDK (Kick-Drift-Kick). El término de fricción comóvil aplicado durante el paso de deriva es:

$$
\mathbf{v} \leftarrow \mathbf{v} \cdot \exp \left(-2 \, E(z) \cdot C_{\mathrm{drag}} \cdot \frac{H_0 t_0}{(H_0 t_0)_{\mathrm{ref}}} \cdot \frac{\Delta t}{\Delta t_{\mathrm{ref}}}\right)
$$

donde $C_{\mathrm{drag}} = 0.25$ se calibra empíricamente por encima del valor puramente físico para tener en cuenta el amortiguamiento de la dispersión de velocidades sub-malla no resuelta.

Para emular las distorsiones observacionales del espacio de corrimiento al rojo (RSD), aplicamos un desplazamiento en la línea de visión (LOS) usando un campo de velocidades híbrido fenomenológico. La densidad local controla con qué fuerza cada trazador usa información de velocidad perturbativa (gran escala) o local (no lineal):

$$
\mathbf{v}_{\mathrm{hyb}}= (1-w_\rho)  \mathbf{v}_{\mathrm{PT}}+w_\rho   \mathbf{v}_{\mathrm{local}}.
$$

$$
w_\rho = w_{\min} + (w_{\max}-w_{\min}) s  \left(\rho/\bar{\rho}\right),
\qquad
w_{\min}=0.15 ; w_{\max}=0.85,
$$

con $s$ elegido como una rampa suave dependiente de la densidad, desde la ponderación PT a baja densidad ($\rho/\bar{\rho}\le 0.5$) hasta la ponderación por velocidad local a alta densidad ($\rho/\bar{\rho}\ge 5.0$).

La transformación al espacio de corrimiento al rojo es entonces:

$$
\mathbf{x}_{\mathrm{RSD}}=\mathbf{x}_{\mathrm{real}}+
A_{\mathrm{RSD}} 
\frac{h  (\mathrm{pxPerMpc})  v_{\parallel \mathrm{hyb}}}{a E(z)} \hat{\mathbf{n}},
$$

con $A_{\mathrm{RSD}}$ la amplitud controlada por el usuario, $\hat{\mathbf{n}}$ el vector unitario de la LOS y $E(z)=H(z)/H_0$.

Cuando se mueve el deslizador temporal manual mientras está en pausa, las velocidades locales se reestiman brevemente en posiciones fijas para mantener continua la vista RSD.


### 4. Implementación: CPU frente a WebGPU

El motor físico incluye dos backends de gravedad para garantizar un alto rendimiento en todos los dispositivos:

| Característica | Modo CPU | Modo GPU (WebGPU) |
| :--- | :--- | :--- |
| **Técnica** | Malla de hash espacial | Compute shaders |
| **Patrón de búsqueda** | Celdas $3 \times 3$ de la malla | Radio circular ($r \le 4$) |
| **Celdas exploradas** | 9 | ~49 ($x^2 + y^2 \le 16$) |
| **Tubería** | Síncrona | Asíncrona (doble búfer) |

**Tubería asíncrona en GPU:** Durante la reproducción en vivo, el *compute shader* de WebGPU usa lectura asíncrona con *staging* de doble búfer para eliminar los bloqueos de la tubería. El fotograma $N$ lanza el pase de cómputo mientras lee simultáneamente el resultado del fotograma $N-1$, introduciendo una latencia despreciable de 1 fotograma. Durante la grabación de vídeo, la tubería cambia automáticamente al modo síncrono para garantizar resultados deterministas y perfectos fotograma a fotograma.


### 5. Búsqueda de vacíos y análisis estructural

La identificación de vacíos en tiempo real se delega a un Web Worker en segundo plano. El motor implementa una metodología fuertemente inspirada en herramientas cosmológicas estándar como [ZOBOV](https://ui.adsabs.harvard.edu/abs/2008MNRAS.386.2101N/abstract) y [VIDE](https://ui.adsabs.harvard.edu/abs/2015A%26C.....9....1S/abstract).

* **Estimación de densidad (VTFE):** La simulación calcula la triangulación de Delaunay y las celdas de Voronoi correspondientes para todos los trazadores. La densidad local se estima mediante el área inversa de la celda: $\rho_i = \overline{A}/A_i$, donde $\rho = 1$ es la densidad media y $\overline{A}$ el tamaño medio de celda.
* **Algoritmo de cuencas (Watershed):** El campo se segmenta encontrando los mínimos locales de densidad. Las celdas adyacentes fluyen «cuesta abajo» hacia esos mínimos, agrupando el universo en cuencas de vacíos distintas.
* **Fusión automática de crestas:** El algoritmo analiza la frontera compartida (crestas) entre vacíos adyacentes. Si más del 20% de una frontera compartida (y al menos 4 pares de celdas en recuento absoluto) consiste en paredes con densidad inferior a la media cósmica, los vacíos se fusionan. Esto evita la fragmentación artificial causada por el ruido de Poisson.
* **Fusión adicional:** Los vacíos pequeños ($R < R_{min}$) pueden absorberse iterativamente en su vecino más grande. Todos los cálculos topológicos respetan las fronteras periódicas mediante un búfer de «partículas fantasma» con envoltura.


### 6. Clasificación de la red cósmica (T-web 2D)

La superposición clasifica cada celda de la malla a partir de la estructura de autovalores del tensor de marea, siguiendo la prescripción estándar T-web ([Hahn et al. 2007](https://ui.adsabs.harvard.edu/abs/2007MNRAS.375..489H/abstract), [Forero-Romero et al. 2009](https://ui.adsabs.harvard.edu/abs/2009MNRAS.396.1819F/abstract)) tal como se implementa en la referencia 3D `webjax` (`cwc.py`), reducida a dos dimensiones.

* **Construcción del campo.** Las posiciones de los trazadores se asignan a una malla periódica ($N_g = 2^n$, 64--512; por defecto 256) con deposición CIC para obtener $\delta = \rho/\bar{\rho} - 1$, y luego se suavizan gaussianamente en el espacio de Fourier con $\exp(-k^2 R^2/2)$. Por defecto se usan las posiciones de *evolución* (espacio de análisis), de modo que la superposición no queda distorsionada por los desplazamientos del espacio de corrimiento al rojo.
* **Base de potencial o de densidad.** Con la base de potencial $\hat{\phi} = -\hat{\delta}/k^2$; con la base de densidad $\hat{f} = -\hat{\delta}$, es decir, se invierte el signo para que en ambos casos los autovalores *positivos* signifiquen colapso a lo largo de ese eje. Las dos bases dan la misma topología de clasificación (difieren en la ponderación $k^4$ del espectro), lo que las pruebas unitarias confirman con precisión de máquina sobre campos analíticos de un solo modo.
* **Hessiano y autovalores.** El hessiano $f_{ij}$ se evalúa con diferencias centrales periódicas en unidades físicas ($\mathrm{Mpc}/h$) y se diagonaliza analíticamente, $\lambda_{1,2} = \tfrac{f_{xx}+f_{yy}}{2} \pm \sqrt{(\tfrac{f_{xx}-f_{yy}}{2})^2 + f_{xy}^2}$ con $\lambda_1 \geq \lambda_2$.
* **Clasificación.** El umbral se expresa en unidades de la dispersión, $\nu = \text{umbral} \cdot \sigma(\lambda_1)$, y una banda de «corrección de crestas» de $\pm 0.1\sigma$ suprime las clasificaciones erróneas inducidas por ruido en la frontera. Las celdas con $\lambda_2 \geq \nu$ son **nudos** (dos ejes colapsados), las que tienen $\lambda_1 \geq \nu > \lambda_2$ son **filamentos** (un eje colapsado) y el resto son **vacíos**.
* **2D frente a 3D.** La reducción 2D tiene tres clases, no cuatro: la clase *sheet* (lámina) 3D (exactamente un eje colapsado) es el análogo del filamento y aquí no es identificable por separado. La escala de suavizado está en $h^{-1}\mathrm{Mpc}$ y debería superar la separación media entre trazadores; el valor por defecto ($R = 6\ h^{-1}\mathrm{Mpc}$) es un compromiso entre el nivel de ruido de un conjunto finito de trazadores y la definición de la red.

La clasificación cuesta aproximadamente 10--40 ms en $256^2$ y se almacena en caché, recalculándose solo cuando cambian el campo, el suavizado, el umbral, el corrimiento al rojo o la clave del espacio de análisis (limitado a ~350 ms mientras la simulación se está reproduciendo). Los colores son semitransparentes para que los trazadores sigan siendo visibles; la misma leyenda discreta se dibuja sobre el lienzo para que sobreviva a capturas y grabaciones.

### 7. Dispersión térmica en tiempo de renderizado

Para representar la dispersión de velocidades sub-malla (virialización) de los halos de materia oscura sin el coste computacional de órbitas reales de N-cuerpos de alta resolución, el motor aplica un desplazamiento cosmético modulado físicamente en el momento del renderizado. La amplitud de la dispersión viene dada por cinco factores:

* **Amplitud dependiente de la densidad:** La amplitud tiende a un valor de referencia en los vacíos y se suprime en las estructuras densas:

$$
\sigma \propto \frac{1}{1 + \alpha (\rho / \bar{\rho})}
$$

* **Elongación anisótropa:** La dispersión se estira a lo largo del vector de fuerza 1LPT local para imitar el flujo de partículas a lo largo de los filamentos.
* **Escalado con la tasa de crecimiento:** Escalada por $f(a)^{0.5}$, donde $f(a) \approx \Omega_m(a)^{0.55}$, para representar la termalización de la energía cinética en épocas cósmicas tardías.
* **Ponderación por cruce de cáscaras:** La dispersión se incrementa en las regiones multi-flujo para representar visualmente las zonas donde ha ocurrido relajación violenta.
* **Fases coherentes persistentes:** Los desplazamientos aleatorios se derivan de una tabla precalculada de seno/coseno basada en un ángulo de fase persistente, lo que garantiza una deriva orbital suave en lugar de ruido brusco entre fotogramas.




---


## Referencias / Bibliografía

* **Eisenstein, D. J., & Hu, W. (1998).** *Baryonic Features in the Matter Transfer Function.* [NASA ADS](https://ui.adsabs.harvard.edu/abs/1998ApJ...496..605E/abstract)
  > Usado para generar las condiciones iniciales rigurosas de $P(k)$ 3D.
* **Chevallier, M., & Polarski, D. (2001).** *Accelerating universes with scaling dark matter.* [NASA ADS](https://ui.adsabs.harvard.edu/abs/2001IJMPD..10..213C)
  > Base de la parametrización de energía oscura dinámica ($w_0w_a$CDM).
* **Linder, E. V. (2005).** *Cosmic growth history and expansion history.* [NASA ADS](https://ui.adsabs.harvard.edu/abs/2005PhRvD..72d3529L/abstract)
  > Usado para escalar la constante de acoplamiento gravitatorio efectiva $G_{\mathrm{eff}}$.
* **Neyrinck, M. C. (2008).** *ZOBOV: a parameter-free void-finding algorithm.* [NASA ADS](https://ui.adsabs.harvard.edu/abs/2008MNRAS.386.2101N/abstract)
  > Base teórica del estimador de campo por teselación de Voronoi (VTFE) y de la segmentación por cuencas.
* **Sefusatti, E., et al. (2016).** *Accurate Estimators of Correlation Functions in Fourier Space.* [NASA ADS](https://ui.adsabs.harvard.edu/abs/2016MNRAS.460.3624S/abstract)
  > Guió la identificación de vacíos.
* **Sutter, P. M., et al. (2015).** *VIDE: The Void IDentification and Examination toolkit.* [NASA ADS](https://ui.adsabs.harvard.edu/abs/2015A%26C.....9....1S/abstract)
  > Guió la identificación de vacíos.


---


## Agradecimientos

* **Concepto inicial:** Simon Bouchard y Nico Schuster
* **Desarrollo:** Nico Schuster
* **Generación de código:** Google Gemini Pro 3.0/3.1 y Claude Opus 4.5/4.6
* **Bibliotecas principales:** La visualización de búsqueda de vacíos se apoya fuertemente en la excelente biblioteca [d3-delaunay](https://github.com/d3/d3-delaunay) para geometría computacional.

Los autores de este código agradecen a Julien Zoubian y Dennis Frei sus valiosas contribuciones al desarrollo del código, así como a Julian Bautista, Marie-Claude Cousinou, Steffen Hagstotz, Nico Hamaus, Geray Karademir, Arnaud de Mattia, Alice Pisani, Benjamin Racine y Pauline Zarrouk por sus útiles discusiones y comentarios. NS cuenta con el apoyo del plan de inversión France 2030 del gobierno francés (A*MIDEX AMX-22-CEI-03).

## Citación

Si utilizas el Cosmic Web Explorer en tus materiales pedagógicos, presentaciones o investigación, cítalo usando el DOI de Zenodo:

[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.18915566-blue.svg)](https://doi.org/10.5281/zenodo.18915566)

```bibtex
@software{Schuster_CosmicWebExplorer_2026,
  author       = {Schuster, Nico and Bouchard, Simon and Zoubian, Julien and Frei, Dennis},
  title        = {Cosmic Web Explorer: Real-Time Large-Scale Structure in the Browser},
  month        = mar,
  year         = 2026,
  publisher    = {Zenodo},
  version      = {v1.0.0},
  doi          = {10.5281/zenodo.18915566},
  url          = {https://doi.org/10.5281/zenodo.18915566}
}
```

## Privacidad y analítica
El sitio de GitHub Pages de este proyecto usa [GoatCounter](https://www.goatcounter.com/), una herramienta de analítica web de código abierto respetuosa con la privacidad. Ayuda a ver cuántas personas visitan el sitio sin rastrear datos personales ni usar cookies.


## Licencia y ciencia abierta

Este proyecto está licenciado bajo la **GNU Affero General Public License v3.0 (AGPLv3)**.

Elegimos esta licencia no para restringir, sino para asegurar que el **Cosmic Web Explorer** siga siendo un recurso permanente y gratuito para la comunidad astronómica. Al usar la AGPLv3 garantizamos que:
* **Acceso abierto:** El código permanecerá siempre de código abierto.
* **Mejoras compartidas:** Si mejoras la física o la interfaz, esas mejoras quedan en la comunidad para que todos se beneficien.
* **Atribución:** Tu trabajo (y el nuestro) está protegido. Los créditos originales deben permanecer siempre intactos.

**En resumen:** siéntete libre de hacer un fork, romperlo y construir algo mejor, ¡solo mantén las luces encendidas para el siguiente!
