# Proyecto: Sistema de Alarmas (Patrones State y Observer)

Este proyecto simula el funcionamiento de un sistema de alarmas, con una `Central` y múltiples `Sensores`. Es un ejercicio práctico para implementar dos patrones de diseño clave: **State** y **Observer**.

## El Problema

Teníamos que modelar un sistema donde:
1.  Los sensores (`Movimiento`, `Contacto`) tienen tres estados: `Apagado`, `Esperando` y `Alerta`.
2.  La `Central` debe ser *notificada* inmediatamente cuando un sensor entra en estado de `Alerta`.
3.  La `Central` también *controla* el estado de los sensores (los activa, desactiva o resetea).
4.  El comportamiento de un sensor cambia drásticamente según su estado (ej: no puede detectar si está `Apagado`).

## La Solución (Los Patrones)

Usamos dos patrones para resolver esto de forma limpia:

### 1. Patrón State

En lugar de tener un `if (estado == "Alerta") ... else if (estado == "Esperando")...` gigante dentro de la clase `Sensor`, usamos el patrón State.

* Creamos una interfaz `EstadoSensor` (`Apagado`, `Esperando`, `Alerta`).
* Cada estado es su propia clase y contiene la lógica de *qué pasa en ese estado*.
* El `Sensor` simplemente "guarda" cuál es su estado actual y le delega el comportamiento (ej: `sensor.activar()`).
* Cuando un sensor cambia de estado, simplemente cambia su objeto `Estado` interno por uno nuevo.

### 2. Patrón Observer

Para que la `Central` se entere de las alertas sin tener que estar preguntando a cada sensor ("¿estás bien? ¿y vos?"), usamos el patrón Observer.

* El `Sensor` es el "Sujeto" (Observable).
* La `Central` es el "Observador" (Observer).
* La `Central` se *suscribe* a todos los sensores que le importan.
* Cuando un `Sensor` entra en estado de `Alerta`, llama a su método `notificar()` y la `Central` recibe el aviso automáticamente (y guarda el evento en su historial).

Esto desacopla el sistema: podemos agregar nuevos tipos de sensores sin que la `Central` tenga que cambiar, siempre y cuando el sensor sepa cómo notificar a sus observadores.
