# Análisis operacional en servidores

## Introducción: Redes de Colas de Espera

**Red de colas**: Conjunto de estaciones de servicio conectadas entre sí.

**Estación de servicio (service station)**: Objeto abstracto compuesto por
un dispositivo (recurso físico) que presta un servicio y una cola de espera
para los trabajos (clientes) que demandan un servicio de él.

**Modelo de servidor central**: Cada trabajo que abandona el servidor(=proceso ejecutado) ha "visitado" la CPU tantas veces como la suma de sus “visitas” a cada uno los dispositivo de E/S más 1.

Es el modelo de redes de colas que más se ha utilizado para representar el comportamiento de los programas en un servidor de cara a extraer información sobre su rendimiento.
¿Cuál es este comportamiento?
  1. Un trabajo que “llega” al servidor comienza utilizando el procesador.
  2. Después de “abandonar” el procesador, el trabajo puede:
    1. terminar (sale del servidor), o bien
    2. realizar un acceso a una unidad de entrada/salida (discos, red,...).
Después de una operación con una unidad de entrada/salida, el trabajo vuelve a “visitar” al procesador.

Recursos considerados
  - Procesador o procesadores.
  - Entrada/salida: unidades de disco, red, etc.

**Variables temporales para la estación**:
  - Tiempo de espera en cola (W, waiting time): Tiempo transcurrido desde que el trabajo solicita hacer uso del recurso físico hasta que realmente empieza a utilizarlo.
  - Tiempo de servicio (S, service time): Tiempo transcurrido desde que el trabajo hace uso del recurso físico hasta que lo libera.
  - Tiempo de respuesta de la estación de servicio (R, response time). Suma de los dos tiempos anteriores.

**Estaciones con más de un servidor**: Son capaces de atender a más de un trabajo en paralelo poseen un parámetro:
  - **Z** (think time): representa el tiempo que requiere el usuario antes de volver a lanzar una petición al servidor tras la respuesta de éste.

**Tipos de colas:**
  - Abiertas: Los trabajos llegan a la red a través de una fuente externa que no controlamos. Tras ser procesados, salen de ella a través de uno o más sumideros. El número de trabajos en el servidor (N_0) puede variar con el tiempo.
  - Cerradas: Presentan un número constante de trabajos que van recirculando por la red (N_T). Dependiendo de si hay o no interacción con usuarios se distingue entre redes de tipo batch (por lotes) o redes interactivas.
  - Mixtas: Cuando el modelo no corresponde a ninguno de los dos anteriores.

## Variables y leyes operacionales

**Análisis operacional**: Técnica de análisis de redes de colas presentada por Denning y Buzen en 1978. Basada en valores medios de variables operacionales (=magnitudes medibles) del sistema informático.

**Razón de visita**: Representa la proporción entre el número de trabajos completados por el servidor y el número de trabajos completados por la estación de servicio i-ésima (dispositivo). Es como si el trabajo tuviera que “visitar” el dispositivo i-ésimo una media de V i veces antes de poder abandonar el servidor.

**Demanda de servicio**: Cantidad de tiempo que, por término medio, el dispositivo i-ésimo dedica a cada trabajo que abandona el servidor.

**Ley de Little**: relaciona el número medio de trabajos en el servidor (N_0) con el tiempo medio de respuesta (R_0 ) y su tasa de llegada (λ_0 ). La ley de Little puede ser aplicada no solo al servidor en su totalidad, sino a cada estación de servicio y a cada uno de los diferentes sub- niveles de una estación de servicio.

**Ley de la Utilización**: Relaciona la utilización de un dispositivo con el número de trabajos que es capaz de realizar por unidad de tiempo (=su productividad) y el tiempo que le dedica a cada uno de ellos (=su tiempo de servicio). Para un sistema en equilibrio de flujo (servidor no saturado), se podía haber deducido también la ley de la utilización sin más que aplicar la ley de Little al recurso físico (dispositivo) de una estación:

**Ley de flujo forzado**: relaciona la productividad del servidor con la de cada uno de los dispositivos que integran el mismo. Como consecuencia de la ley del flujo forzado, las utilizaciones de cada dispositivo son proporcionales a las demandas de servicio del mismo, siendo la constante de proporcionalidad precisamente la productividad global del servidor

**Ley general del tiempo de respuesta**: i el servidor no está saturado y teniendo en cuenta que el número medio de trabajos dentro de un servidor no es más que la suma del número medio de trabajos que hay en cada estación de servicio

**Ley del tiempo de respuesta interactivo**: Se obtiene mediante la aplicación de la ley de Little a una red cerrada de tipo interactivo. Una red cerrada nunca se satura (si el tamaño de las colas es suficiente).

## Límites optimistas del rendimiento

**Cuello de botella**: Todo servidor presenta alguna limitación en su rendimiento. En esta sección veremos que la localización del elemento limitador no solo depende del servidor sino también de la carga. Al elemento limitador del rendimiento del servidor se le denomina cuello de botella. eremos que la única manera de mejorar las prestaciones de un servidor de manera significativa es actuando sobre el cuello de botella.

**Identificación del cuello**: El cuello de botella es el dispositivo con mayor utilización ya que será el primero que llegue a saturarse cuando aumente la carga. Podemos identificar fácilmente el cuello de botella de un servidor simplemente buscando el dispositivo con mayor demanda de servicio.

**Servidor equilibrado**: Servidor en que todos los dispositivos tienen la misma demanda de servicio o utilización (la carga se absorbe equitativamente)

## Técnicas de mejora

**Sincronización o ajuste**: Optimización del funcionamiento de componentes existentes:
  - Componentes hardware: frecuencias, voltajes, parámetros de la placa, ...
  - Aplicaciones. Usamos profilers.
  - Sistema operativo: políticas de gestión de procesos y memoria virtual, distribución de la información entre discos

**Actualización o ampliación**: Reemplazar dispositivos por otros más rápidos  Disminuimos el
tiempo medio de servicio. Procesador, memoria, placa base, disco...Añadir dispositivos para poder realizar más tareas en paralelo  Disminuimos la razón de visita. Ejemplo: multiprocesadores, matrices de discos (RAID)

## Algoritmos de resolución de modelos de redes de colas
