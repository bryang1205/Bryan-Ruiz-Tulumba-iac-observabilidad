## Preguntas del laboratorio


### a. ¿Por qué necesitamos Loki además de Prometheus si ya tenemos /metrics?

Prometheus y /metrics solo manejan números como porcentaje de CPU. Loki existe para los logs, que son eventos con contexto: error en un stack, el mensaje de una excepción, o detalle de una transacción fallida. Prometheus nos dice cuando algo ya salió mal (la métrica), y Loki qué salió mal (el log del error).

### b. ¿Qué ventaja aporta que las fuentes de datos de Grafana estén aprovisionadas como código y no creadas a mano?

Que es reproducible y versionable. Si se destruye el contenedor de Grafana y se vuelve a crear, las fuentes de datos de Prometheus y Loki aparecen sin intervención manual (como clics). En equipo, todos tienen el mismo entorno. Además, los cambios quedan en Git, permitiendo auditar cambios y versiones.

### c. El panel "CPU contenedor" y el panel "CPU host" pueden mostrar valores muy distintos. ¿Por qué? ¿Cuál usarías para alertar sobre una aplicación concreta?

El panel del CPU host muestra el uso de la máquina, sumando los procesos del SO, Docker,  los contenedores y cualquier proceso corriendo. El CPU contenedor muestra solo lo que consume ese contenedor específico. Pueden variar si hay procesos pesados en el host. Para alertar sobre una aplicación concreta usaría el panel de CPU por contenedor, porque refleja el comportamiento de esa app sin ruido externo.

### d. ¿Qué diferencia hay entre el evaluation interval y el pending period de una alarma?

El evaluation interval es cada cuánto Grafana evalúa la condición de la alarma, por ejemplo cada 10 segundos. El pending period es cuánto tiempo debe mantenerse la condición de forma continua antes de que la alarma pase a estado Firing, por ejemplo 30 segundos. Evaluation interval controla la frecuencia de la revisión, y el pending period evita falsos positivos: un pico de CPU de 2 segundos no dispara la alarma si el pending period es 30 segundos.