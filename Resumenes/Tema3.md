# Monitorización de servicios y programas

## Concepto de Monitor de actividad

 - Carga (workload): conjunto de tareas que ha de realizar un sistema. (= Todo aquello que demande recursos del sistema.)
 - Actividad de un sistema: conjunto de operaciones que se realizan en el sistema como consecuencia de la carga que soporta.

Un sistema informático no es “bueno” ni “malo” per se, sino que se adapta mejor o peor a un tipo determinado de carga. Algunas variables que reflejan la actividad de un sistema:

  - **Procesadores**: Utilización, temp., f CLK , no de procesos, no de interrupciones, cambios de contexto, etc.
  - **Memoria**: no de accesos, memoria utilizada, fallos de caché, fallos de página, uso de memoria de intercambio, latencias, anchos de banda, voltajes, etc.
  - **Discos**: lecturas/escrituras por unidad de tiempo, longitud de las colas de espera, tiempo de espera medio por acceso, etc.
  - **Red**: paquetes recibidos/enviados, colisiones por segundo, sockets abiertos, paquetes perdidos, etc.
  - **Sistema global**: no de usuarios, no de peticiones, etc.

**Monitor de actividad**: Herramienta diseñada para medir la actividad de un sistema informático y facilitar su análisis. Las acciones típicas deun monitor:
  - Medir la actividad.
  - Procesar y almacenar la información recopilada.
  - Mostrar los resultados.

**Utilidad de los monitores** según el punto de vista de cada ente:
  - **Administrador/Ingeniero**:
    - Conocer la utilización de los recursos (detección de cuellos de botella) para saber: Qué hardware hay que reconfigurar / sustituir/ añadir. Cómo ajustar los parámetros del sistema (sintonización).
    - Predecir cargas futuras (capacity planning).
    - Tarificar a los clientes.
    - Obtener un modelo un componente o de todo el sistema para poder deducir qué pasaría si...
  - **Programador**: Conocer las partes críticas de una aplicación de cara a su optimización.
  - **Sistema Operativo**: Adaptarse dinámicamente a la carga.

**Tipos de monitores**:

  - **Por eventos**: Evento: Cambio en el estado del sistema. Volumen de información recogida: Depende de la frecuencia de los eventos. Información exacta. Ejem.: Inicio/fin de la ejecución de un programa, fallo en memoria cache, interrupción de un dispositivo periférico.

  - **Por muestreo** (A intervalos regulares de tiempo): Volumen de información recogida: Depende del periodo de muestreo (T). Información estadística.

  - **Software**: Programas instalados en el sistema.
  - **Hardware**: Dispositivos físicos de medida (menor sobrecarga)
  - **Híbridos**: Utiliza los dos tipos anteriores

**¿Existe interacción con el analista/administrador?**
  - **Sí existe**. Durante el propio proceso de monitorización se pueden consultar los valores monitorizados y/o interactuar con ellos realizando representaciones gráficas diversas, modificando parámetros del propio monitor, etc.: monitores en primer plano o interactivos (on-line monitors).

  - **No existe**. La consulta sobre los resultados se realiza aparte mediante otra herramienta independiente al proceso de monitorización: monitores tipo batch, por lotes o en segundo plano (batch monitors).

**Atributos que caracterizan a un sensor/monitor**:
  - **Exactitud** de la medida (Accuracy, offset): ¿Cómo se aleja el valor medido del valor real que se quiere medir?
  - **Precisión** (Precision): ¿Cuál es la dispersión de las medidas?
  - **Resolución del sensor**: ¿Cuánto tiene que cambiar el valor a medir para detectar un cambio?
  - **Tasa Máxima de Entrada** (Max Input Rate): ¿Cuál es la frecuencia máxima de ocurrencia de los eventos que el monitor puede observar? (monitores por eventos)
  - **Periodo de Muestreo** (Sampling Time): ¿Cada cuánto tiempo tomamos la medida? (monitores por muestreo)
  - **Anchura de Entrada** (Input Width): ¿Cuánta información (no de bytes de datos) se almacena por cada medida que toma el monitor?
  - **Sobrecarga (Overhead)**: ¿Qué recursos le “roba” el monitor al sistema? El instrumento de medida puede perturbar el funcionamiento del sistema. **Sobrecarga_Recurso(%) = Uso del recurso / Capacidad total recurso x 100**

## Monitor a nivel de sistema

**El directorio /proc**: Es una carpeta en RAM utilizada por el núcleo de Unix para facilitar el acceso del usuario a las estructuras de datos del S.O. A través de /proc podemos:
  - **Acceder a información global sobre el S.O.** : loadavg, uptime, cpuinfo, meminfo, mounts, net, kmsg, cmdline, slabinfo, filesystems, diskstats, devices, interrupts, stat, swap, version, vmstat ...
  - **Acceder a la información de cada uno de los procesos del sistema **(/proc/[pid]): stat, status, statm, mem, smaps, cmdline, cwd, environ, exe, fd, task...
  - **Acceder y, a veces, modificar algunos parámetros del kernel del S.O**. (/proc/sys): dentry_state, dir-notify-enable, dquot-max, dquot-nr, file- max, file-nr, inode-max, inode-nr, lease-break-time, mqueue, super- max, super-nr, acct, domainname, hostname, panic, pid_max, version, net, vm...

**uptime**: Tiempo que lleva el sistema en marcha y la “carga media” que soporta donde podemos observar cuando lo ejecutamos por terminal: hora actual, tiempo en marcha, carga media en último minuto, últimos 5 y últimos 15.

**ps**: Información sobre el estado actual de los procesos del sistema. Se pueden seleccionar multitud de campos diferentes que mostrar sobre cada proceso y diferentes formas de ordenar la lista resultante.

  - **Procesos a mostrar**: -A, -e: show all processes; T: all processes on this terminal U: processes for specified users...
  - **Orden de presentación**: -C: sort by command name, -p sort by process ID...

**La información de ps** (significado de las columnas):  
  - USER: Usuario que lanzó el proceso
  - %CPU, %MEM: Porcentaje de procesador y memoria física usada
  - SIZE (o VSIZE): Memoria (KiB) virtual total asignada al proceso
  - RSS (resident set size): Memoria (KiB) física ocupada por el proceso
  - STAT:
    - R (running or runnable), D (I/O blocked), S (interruptible sleep), T (stopped), Z (zombie: terminated but not died)
    - N (lower priority = running niced), < (higher priority = not nice)
    - s (session leader), + (in the foreground process group), W (swapped)

**top**: Muestra cada T segundos: carga media, procesos, consumo de memoria... Normalmente se ejecuta en modo interactivo (se puede cambiar T, las columnas seleccionadas, la forma de ordenar las filas, etc.)

**vmstat**: Paging (paginación), swapping, interrupciones, cpu. Con otros argumentos, puede dar información sobre acceso a discos (en concreto la partición de swap) y otras estadísticas de memoria.

**Paquete systat:**

**sar** (system activity reporter): Es un monitor de actividad utilizado por los administradores de sistemas Unix para, entre otras cosas, la detección de cuellos de botella (bottlenecks). Información sobre todo el sistema. Actual, de hoy o justo en el instante e historica guardad en un log llamada /var/log/systat/saDD donde DD es dia y mes. Hace uso de contadores estadísticos del núcleo del sistema operativo ubicados en el directorio /proc.

El funcionamiento de sar se base en dos ordenes complementarias:
  - **sadc** (system-accounting data collector) Recoge los datos estadísticos (lectura de contadores) y construye un registro en formato binario (back-end)
  - **sar**: Lee los datos binarios que recoge sadc y las traduce a un formato legible por nosotros en formato texto (front-end)
  - Otras herramientas de sysstat:
    - **mpstat** (processors related statistics). Nos muestra estadísticas concretas por cada núcleo (core). Podemos ver si la carga está bien distribuida entre los diferentes núcleos.
    - **iostat**: (input/output statistics). Muestra estadísticas de dispositivos de E/S y particiones de disco existentes en el sistema.

**Procedimiento sistemático de monitorización** método USE (Utilization, Saturation, Errors) para cada recurso (CPU, memoria, E/S, red,...) comprobamos:
  - **Utilización**: Tanto por ciento de utilización del recurso (tiempo de CPU, tamaño de memoria, ancho de banda, etc.)
  - **Saturación**: Tamaño de las colas de aquellas tareas que quieren hacer uso de ese recurso (en el caso de la memoria, cantidad de swapping a disco).
  - **Errores**: Mensajes de error del kernel sobre el uso de dichos recursos

Otras herramientas de monitorización:
  - CollectL
  - Nagios
  - Ganglia
  - Munin
  - Zabbix

**SarCheck**: Herramienta para el análisis de prestaciones y sintonización, planificación de la capacidad. Genera informes en formato HTML: Estadísticas sobre el uso de recursos. análisis de recursos, detectando posibles cuellos de botella, sección de recomendaciones. Basado en sar.


## Monitor a nivel de aplicación

**Monitorización a nivel de aplicación (profiling)**: Su objetivo es Observar el comportamiento de una aplicación concreta con el fin de obtener información para poder optimizar su código. Información que pueden proporcionar las herramientas de análisis de
aplicaciones (profilers):
  - ¿Cuánto tiempo tarda en ejecutarse el programa? ¿Qué parte de ese tiempo es de usuario y cuál de sistema? ¿Cuánto tiempo se pierde por las operaciones de E/S?
  - ¿En qué parte del código pasa la mayor parte de su tiempo de ejecución
  - ¿Cuántas veces se ejecuta cada línea de programa?
  - ¿Cuántas veces se llama a un procedimiento y desde dónde?
  - etc.

**time**: El programa /usr/bin/time mide el tiempo de ejecución de un programa y muestra algunas estadísticas sobre su ejecución.
  - **real**: tiempo total usado por el sistema (wall-clock CPU time).
  - **user**: tiempo de CPU ejecutando en modo usuario.
  - **sys**: tiempo de CPU ejecutando código del núcleo.
  - **Cambios de contexto voluntarios**: al tener que esperar a una operación de E/S cede la CPU a otro proceso.
  - **Cambios involuntarios**: Expira su “time slice”.
  - **Major page faults**: requieren acceder al disco.

**Monitor gprof**: Da información sobre el tiempo de CPU y número de veces que se ejecuta cada función de un proceso/hilo en un sistema Unix. Para utilizar gprof compilamos con -pg -g y ejecutamos quedando la info en gmon.out. Para ver la información ejecutamos el programa con gprof reedireccionamos con ">" a prog.gprof.

**Monitor gcov**: Aporta información sobre el número de veces que se ejecuta cada línea
de código del programa. Para utulizar gcov compilamos con fprofile-arcs –ftest-coverage, ejecutamos el programa aunque esta vez la información recogida se deja en varios ficheros. Para la visualización de lla información: gcov prog.c (genera el fichero prog.c.gcov).

**Profile Perf**: es un conjunto de herramientas para el análisis de rendimiento en Linux basadas en eventos software y hardware (hacen uso de contadores hardware disponibles en los últimos microprocesadores de Intel y AMD). Permiten analizar el rendimiento de
  a) un hilo individual,
  b) un proceso + sus hijos,
  c) todos los procesos que se ejecutan en una CPU concreta,
  d) todos los procesos que se ejecutan en el sistema. Algunos de los comandos que proporciona:

**Profile Valgrind**: s un conjunto de herramientas para el análisis y mejora del código. Entre éstas, encontramos:
  - **Callgrind**, una versión más refinada de gprof.
  - **Cachegrind**, un profiler de la memoria caché.
  - **Memcheck**, un detector de errores de memoria.

Valgrind puede analizar cualquier binario ya compilado (no necesita instrumentar el programa a partir de su código fuente). Valgrind actúa, esencialmente, como una máquina virtual que emula la ejecución de un programa ejecutable en un entorno aislado. Como desventaja, el sobrecoste computacional es muy alto. La emulación del programa ejecutable puede tardar decenas de veces más que la ejecución directa del programa de forma nativa.

**Profilers V-Tune y CodeXL**: Al igual que Perf pueden hacer uso tanto de eventos software como hardware. Ambos programas funcionan tanto para Windows como para Linux, y permiten obtener información sobre los fallos de caché, fallos de TLB, bloqueos/rupturas del cauce, fallos en la predicción de saltos, cerrojos y esperas entre hebras, etc. asociados a cada línea del programa (tanto en código fuente como en código ensamblador).


























 -
