# Análisis comparativo del rendimiento

## Índices clásicos de rendimiento

**Características de un buen rendimiento**

  - **Representatividad y fiabilidad**: Si un sistema A siempre presenta un índice de rendimiento mejor que el sistema B, es porque siempre el rendimiento real de A es mejor que el de B.
  - **Repetibilidad**: Siempre que se mida el índice en las mismas condiciones, el valor de éste debe ser el mismo.
  - **Consistencia y facilidad de medición**: El índice debe ser fácil de medir y la forma de medirlo debe ser la misma para cualquier sistema.
  - **Linealidad**: Si el índice de rendimiento aumenta, el rendimiento real del sistema debe aumentar en la misma proporción.

**Tiempo de ejecución, frecuencia de reloj y ciclos por instrucción**

El **tiempo de ejecución** es el mejor índice de rendimiento a priori pero depende del programa o programas que se ejecuten (=la carga que se use). ¿Existen otros índices posibles para expresar el rendimiento del sistema? Históricamente se han usado f_RELOJ , CPI, MIPS y MFLOPS:

T_Ejec = NI x CPI x T_Reloj = (NI x CPI) / f_Reloj

Desventajas: Ni la frecuencia de reloj ni los ciclos por instrucción son representativos del rendimiento de un sistema. Es posible encontrar ejemplos de sistemas con f_RELOJ (o CPI) peores que otros pero con mejores prestaciones.

**MIPS** (million of instructions per second)
  - **Nativos**: Depende del juego de instrucciones (ej. RISC vs CISC).  Además, los MIPS medidos varían incluso entre programas en el mismo computador MIPS: Meaningless Indicator of Processor Speed
  - **Relativos**: referidos a una máquina de referencia para un programa dado (proceso de normalización) -> depende del programa elegido.

**MFLOPS** (million of floating-point operations per second): Basado en operaciones y no en instrucciones.
  - **Problema 1**: No todas las operaciones de coma flotante tienen la misma complejidad -> MFLOPS normalizados: Cada operación se multiplica por un peso que es proporcional a su complejidad.
  - **Problema 2**: El formato de los números en coma flotante puede variar de una arquitectura a otra y, por tanto, los resultados de las operaciones tener diferente exactitud. Además, ¿y si no necesito las operaciones en coma flotante en mi servidor?
  - **Conclusión**: Tampoco es el índice que buscamos y no hay más candidatos. Nos contentaremos con comparar el rendimiento para una carga determinada.

## Benchmarking

**La carga real**: Difícil de utilizar en la evaluación de sistemas. Varía a lo largo del tiempo. Resulta complicado reproducirla. Interacciona con el sistema informático. Es más conveniente utilizar un modelo de la carga real como carga de prueba (test workload) para hacer comparaciones.

**Modelo de carga**: son aproximaciones que representan una abstracción de la carga que recibe un sistema informático. El modelo de la carga:
  - Debe ser lo más representativo posible de la carga real.
  - Debe ser lo más simple/compacto que sea posible (tiempos de medición y espacio en memoria razonables).

**Estrategias de los modelos**:
  - Ajustar un modelo paramétrico **personalizado** a partir de la monitorización del sistema ante la carga real (caracterización de la carga).
  - Usar programas de prueba que usen un modelo **genérico** de carga lo más similar posible al que se quiere reproducir (referenciación o benchmarking).

**Caracterización de la carga**: usualmente la caracterización de la carga de un sistema se realiza siguiendo los siguientes pasos:
  - Identificación de los recursos que más demande la carga (CPU, memoria, discos, red, etc.)
  - Elección de los parámetros característicos de dichos recursos (utilización de CPU, lecturas/escrituras que hay que hacer en cada disco, lecturas/escrituras a memoria, número de accesos a la red, etc.)
  - Recolección de datos (usando monitores de actividad).
  - Análisis y clasificación de los datos (medias, histogramas, agrupamiento o clustering, etc.).
  - Extracción de los representantes de la carga junto con información estadística sobre su distribución temporal.

**Referenciación (Benchmarking)**: Técnica usada en la comparación del rendimiento de diferentes sistemas informáticos.

Un benchmark (benchmark program) es un programa o un conjunto de programas diseñados con el fin de comparar alguna característica del rendimiento entre equipos informáticos. Hay dos caracte- rísticas principales que definen a un programa de benchmark:
  - La carga de prieba (test workload) específica con la que estresa el sistema evaluado.
  - El conjunto de reglas que se deben de seguir para la correcta ejecucción, obtención y validación de los resultados.

**Tipos de benchmark**

  - **Según la estrategia de medida**:
    - Programas que **miden el tiempo** necesario para ejecutar una cantidad pre-establecida de tareas. La mayoría de benchmarks.
    - Programas que **miden la cantidad** de tareas ejecutadas para un tiempo de cómputo pre-establecido. SLALOM: Mide la precisión de la solución de un determinado problema que se puede alcanzar en 1 minuto de ejecución.
    - Programas que permiten variar **tanto la cantidad de tareas como el tiempo de cómputo** para adaptarlos a cada sistema. HINT: Calcula los límites inferior y superior de una integral hasta que elsistema se quede sin recursos. Medida de rendimiento: QUIPS (qualityimprovements per second).

  - **Según la generalidad del test**:
    - **Microbenchmarks** o benchmarks para componentes: estresan componentes o agrupaciones de componentes concretos del sistema: procesador, caché, memoria, discos, red, procesador+caché, procesador+compilador+memoria virtual, etc.
    - **Macrobenchmarks** o benchmarks de sistema completo o de aplicación real: carga compuesta por un conjunto de aplicaciones, normalmente comerciales, habitualmente utilizadas en algún área, p.ej. e-comercio, servidores web, servidores de ficheros, servidores de bases de datos, sistemas de ayuda a la decisión, paquetes ofimáticos + correo electrónico + navegación, etc.


**SPEC** (Standard Performance Evaluation Corporation): Compuesto por cuatro conjuntos de benchmarks con los que obtener 4 índices de rendimiento distintos
  - SPECspeed®2017 **Integer** (rendimiento en aritmética entera)
  - SPECspeed®2017 **Floating Point** (rendimiento en coma flotante)
  - SPECrate®2017 **Integer** (rendimiento en aritmética entera)
  - SPECrate®2017 **Floating Point** (rendimiento en coma flotante)
    - **Speed**: cuánto tarda en ejecutarse un programa (tiempo de respuesta).
    - **Rate**: cuántos programas puedo ejecutar por unidad de tiempo (productividad).

SPEC evalua procesador, sistema de memoriac ompilador (C, Fortran y C++) además de tener reglas estrictas para validar los resultados. El paquete se distribuye como una imagen ISO que contiene:
  - Código fuente de todos los programas de benchmark.
  - Data sets que necesitan algunos benchmarks para su ejecución.
  - Herramientas varias para compilación, ejecución, obtención de resultados, validación y generación de informes.
  - Documentación, incluyendo reglas de ejecución y de generación de informes.

Índices de prestaciones:
  - **Aritmética entera**: CPU2017IntegerSpeed_peak, CPU2017IntegerRate_peak, CPU2017IntegerRate_base. CPU2017IntegerSpeed_base,
  - **Aritmética en coma flotante**: CPU2017FP_Speed_peak, CPU2017FP_Speed_base, CPU2017FP_Rate_peak, CPU2017FP_Rate_base.

Donde el significado de base y peak es:
  - **Base**: Compilación en modo conservador, es decir, con reglas estrictas para que todos usen las mismas opciones de compilación.
  - **Peak**: Rendimiento pico, permitiendo que cada uno escoja las opciones de compilación óptimas para cada programa.

Cada programa del benchmark se ejecuta 3 veces y se escoge el resultado intermedio (se descartan los 2 extremos). El índice final es la media geométrica de las ganancias en velocidad con respecto a una máquina de referencia.

**Benchmarks de sistema completo TPC** (Transactions Processing Performance Council): Organización sin ánimo de lucro especializada en benchmarks relacionados con comercio electrónico y con bases de datos. Los principales benchmarks son :
  - **TPC-C**: Tipo OLTP (on-line transaction processing). Simula una gran compañía con varios almacenes, cada uno con 100.000 productos y tiene 3000 clientes. Peticiones que involucran acceso a las bases de datos tanto locales como distribuidas.
  - **TPC-E**: Tipo OLTP. Simula una correduría de bolsa en donde hay una única base de datos central. El benchmark es escalable de modo que se pueden simular transacciones de compañías de diversos tamaños.
  - **TPC-H**, TPC-DS: Tipo DS (decision support). Se deben ejecutar consultas altamente complejas a una gran base de datos y analizar enormes volúmenes de datos.


## Análisis de los resultados de un benchmark

**¿Cómo expresar el rendimiento final de un benchmark?**

El rendimiento es una variable multidimensional. Habría de expresarse mediante múltiples índices. Sin embargo, las comparaciones son más sencillas si se usa un único índice de rendimiento (a minimizar o maximizar). ¿Cómo concentrar todos los índices en uno solo? Utilizar la mejor variable que represente el rendimiento. Método habitual de síntesis: uso de algún tipo de media.

  - **Media Aritmética**
    - **Normal**: La máquina más rápida (la que ejecuta los programas del benchmark en menor tiempo) es la de menor media aritmética de los tiempos de ejecución.
    - **Ponderada**: pesos para cada valor según su importancia. Según este criterio, la máquina “más rápida” sería la de mejor tiempo medio ponderado de ejecución. Nótese que esta ponderación depende, en este ejemplo, de la máquina de referencia.
  - **Media geométrica**: cuando las medidas son ganancias en velocidad (speedups) con respecto a una máquina de referencia, este índice mantiene el mismo orden en las comparaciones independientemente de la máquina de referencia elegida. Usado en los benchmarks de SPEC y SYSMARK. Se premian las mejoras sustanciales. No se castigan empeoramientos no tan sustanciales. Debemos ser MUY cuidadosos con las comparaciones y saber qué estamos haciendo realmente.


Intentar reducir un conjunto de medidas de un benchmark a un solo “valor medio” final no es una tarea trivial. La media aritmética de los tiempos de ejecución de un benchmark es una medida fácilmente interpretable e independiente de ninguna máquina de referencia. El menor valor nos indica la máquina que ha ejecutado el conjunto de programas del benchmark en un tiempo menor. La media aritmética ponderada nos permite asignar más peso a algunos programas que a otros. Esa ponderación debería realizarse, idealmente, según las necesidades del usuario. Si se hace de forma dependiente de los tiempos de ejecución de una máquina de referencia, la elección de ésta puede influir significativamente en los resultados. La media geométrica de las ganancias en velocidad con respecto a una máquina de referencia es un índice de interpretación compleja cuya comparación no depende de la máquina de referencia. Premia mejoras sustanciales con respecto a algún programa del benchmark y no castiga al mismo nivel los empeoramientos. Independientemente de qué índice se escoja, un buen ingeniero debería, en primer lugar, determinar si las diferencias entre las diferentes medidas obtenidas son estadísticamente significativas. ¿Qué significa eso?



## Introducción al diseño de experimentos

  - Variable respuesta o dependiente (métrica): El índice de rendimiento que usamos para las comparaciones. P.ej. tiempos de respuesta (R), productividades (X).
  - Factor: Cada una de las variables que pueden afectar a la variable respuesta. P.ej. sistema operativo, tamaño de memoria, tipo de disco
  duro, tipo de procesador, número de microprocesadores, número de cores, tamaño de cada caché, compilador, algún parámetro configurable del S.O., etc.
  - Nivel: Cada uno de los valores que puede asumir un factor. P.ej. para un S.O.: Windows, CentOS, Debian, Ubuntu; para un tipo de disco duro: SATA, IDE, SAS; para un parámetro del sistema operativo: ON, OFF, etc.
  - Interacción: El efecto de un determinado nivel de un factor sobre la variable respuesta puede ser diferente para cada nivel de otro factor. P.ej. el hecho de usar un tipo determinado de S.O. puede afectar a cómo de importante sea usar una mayor cantidad de memoria RAM.

**Tipos de diseños experimentales**

  - **Diseños con un solo factor**: Se utiliza una configuración determinada como base y se estudia un factor cada vez, midiendo los resultados para cada uno de sus niveles. Problema: solo válida si descartamos que haya interacción entre factores. (Test ANOVA)
  - **Diseños multi-factoriales completos**: Se prueba cada posible combinación de niveles para todos los factores. Ventaja: se analizan las interacciones entre todos los factores.
  - **Diseños multi-factoriales fraccionados**: Término medio entre los anteriores. No todas las interacciones se verán reflejadas en los resultados, solo las de las interacciones que se consideren más probables.























-
