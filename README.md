# Examen segundo corte

## Integrantes
- Andres Felipe Jimenez Albornoz
- Miguel Angel Timoté Moya
## Descripción del problema
Hay una empresa de lácteos que se dedica a la producción de derivados de la leche. Esta empresa realiza varios procesos de forma manual. Para el desarrollo del diseño se tiene que tomar en cuenta el proceso de etiquetado de las cajas para posteriormente realizar el alistamiento y despacho. Con esta solución se busca aumentar la producción para 2000 lotes diarios. Por lo tanto, la empresa requiere el diseño e implementación de automatización del proceso mencionado anteriormente. Se deben tener los siguientes requerimientos mínimos:
1.	Automatizar el proceso usando PLC, sensores y actuadores.
2.	Proporcionar el HMI con las etapas del proceso.
3.	Realizar un prototipo funcional.
## Proceso de ideación
Cómo parte del proceso inicial, se consultaron algunas referencias para facilitar el proceso del diseño del prototipo funcional. Se tuvo la idea de realizar el diseño usando una cinta transportadora que siempre estaba en movimiento, ya que usualmente, las cadenas de suministro están en funcionamiento continuo [1]. Con la cinta transportadora siempre funcionando, la siguiente tarea era diseñar la lógica para el funcionamiento del sistema de etiqueta. 

Así que se buscaron algunas referencias de máquinas de etiqueta que existen en el mercado. Nos llamo la atención la máquina etiquetadora triton-júpiter ya que esta presenta el proceso de etiquetado de forma vertical [2]. Este sistema de etiquetado vertical favorecía el proceso con la cinta transportadora y su método de funcionamiento era simple para un futuro prototipado.

Teniendo la idea de la cinta y la etiquetadora. El siguiente paso fue diseñar el proceso de detección de las cajas para la activación de la etiquetadora. Así que se opto por incluir los sensores en medio de la cinta transportadora con un espacio entre ellos para así realizar la activación y desactivación del proceso de etiquetado aprovechando que la cinta transportadora nunca se va a detener.

El siguiente boceto muestra la primera versión de la automatización del proceso industrial:

<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/38eeb9bd-c10f-4abd-97aa-8b75ccfadbff" alt="Boceto" width="600"/>
</p>

## Proceso de simulación

Con el boceto en mente, se procedió a realizar la interfaz gráfica o HMI que evidencie el proceso de automatizado del etiquetado de las cajas. Para este desarrollo se crearon diferentes bloques con una serie de pasos que permiten que la visualización a partir de una serie de requerimientos de operatividad. 

-	Debe haber un sistema que permita iniciar y detener el proceso en caso de accidentes.
-	Debe haber una retroalimentación con luces que permitan la visualización de la activación de sensores o actuadores. Así mismo el estado del sistema.

Para este diseño se uso el diagrama ladder, usando contactos y bobinas [3] para así tener el funcionamiento lógico de todo el sistema y poder visualizarlo a partir de un HMI. 

Para el primer requerimiento, se optó por crear un contacto llamado “START” para iniciar la banda transportadora, ya que este es el primer paso para el funcionamiento del sistema automático. Para detener el sistema, se uso un contacto cerrado llamado “STOP” para cada bloque de ejecución para cortar todo el flujo del sistema. También se pensó el HMI como una serie de 5 pasos secuenciales, así que se crearon 5 variables, desde I0 hasta I4 para controlar cada paso. Mediante un temporizador de retardo a la activación o TON [4], se hizo la consecución de pasos por cad variable creada anteriormente, teniendo en total 5 TONs, cada uno con un tiempo de retardo de 2 segundos.

Para el segundo requerimiento se crearon 4 variables para los leds del sistema. El primer led es un led rojo que visualiza en estado activo, que el sistema esta apagado o inactivo, el segundo led es todo lo contrario, es verde y visualiza el estado activo del sistema. Los 3 últimos leds se usaron para mostrar la activación del sensor 1, el sensor 2 y la máquina etiquetadora.

Las siguientes imagenes evidencian el diagrama empleado para la simulación del sistema con su respectiva explicación:

### Paso 1
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/c5cba17f-1059-4b47-8e5f-aa6d20f12583" alt="Paso 1 HMI" width="1000"/>
</p>
En este bloque se activa el contacto BELT y se pone en set el contacto SYSTEM, se pone en reset el contacto SYSTEMOFF. Los contactos SYSTEM y SYSTEMOFF son las luces led que evidencian el estado activo e inactivo del sistema en el HMI, mientras que BELT es una variable auxiliar para iniciar el bucle del sistema de la cinta transportadora.

### Paso 2
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/ee8cc775-19ec-4ace-8cfd-a6d9a6a17ad4" alt="Paso 2 HMI" width="1000"/>
</p>
Cuando se activa el contacto BELT se pone en set el contacto I0 e IR. I0 es el primer paso de funcionamiento del sistema para los contadores e IR remueve la activación del contacto BELT del paso 1 para que no tenga inconvenientes el bucle desde I0.

### Paso 3
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/e6da698c-2877-423c-bcd6-a7f257305a08" alt="Paso 3 HMI" width="1000"/>
</p>
Con el contacto I0 activo, cuando pasan 2 segundos, se pone en set el contacto I1 para la siguiente secuencia, la bobina S1 ya que es el led del primer sensor, la bobina IC ya que es la luz led de la máquina etiquetadora. Se pone en reset el contacto I0.

### Paso 4
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/dcaba0c6-1ce8-4023-82fe-d367b0c0d067" alt="Paso 4 HMI" width="1000"/>
</p>
La misma lógica se usa para este paso. Solo que en esta estación se apaga el sensor 1 y el led de la máquina etiquetadora se mantiene encendida. Además, se usa como contacto para llevar el conteo de la máquina.

### Paso 5
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/acbc1c82-5888-43e8-bf5a-6dd7953cd49f" alt="Paso 5 HMI" width="1000"/>
</p>
Este paso es similar al paso 3. La diferencia radica en que se enciende el sensor 2 y se apaga la máquina etiquetadora.

### Paso 6
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/8ce1b91b-68be-4ebf-8584-0e08d50413e9" alt="Paso 6 HMI" width="1000"/>
</p>
Este paso es igual al paso 2, solo que se apaga el sensor 2. Además, hay un bloque extra que lleva el conteo desde que la caja esta en la estación tres hasta que vuelve a iniciar el ciclo de nuevo.

### Paso 7
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/618abaa4-96bf-4ee0-adb4-bffbd677e10d" alt="Paso 7 HMI" width="1000"/>
</p>
Este bloque es el que reinicia todo el proceso desde el paso 3. Además, el otro bloque lleva el conteo por cada caja que pasa por la cinta transportadora de inicio a fin.

### Paso 0
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/c5ed20e6-8447-46a8-9543-7923f9394d56" alt="Paso 0 HMI" width="1000"/>
</p>
Este bloque simplemente reinicia todas las variables importantes a partir del contacto STOP para volver a empezar todo el sistema desde el paso 1 al presionar de nuevo el contacto START.

### --
Con el diagrama Ladder hecho para todas las etapas del proceso de automatización se diseño la siguiente interfaz gráfica:
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/4efc2471-2a39-4931-83c4-f58cf9eaa6ed" alt="Diagrama HMI" width="1000"/>
</p>

### Estado inicial del HMI
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/9a652a4b-d79e-4a54-bdc9-5061e9247956" alt="Inicial HMI" width="600"/>
</p>

### Primeros 3 estados
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/a958aaa0-6810-4921-912d-757448d068e2" alt="Estado 1" width="300"/>
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/a340f852-a4b2-4abc-83ac-226d6060df4f" alt="Estado 2" width="295"/>
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/92ffc134-7ca4-4442-83a2-6794e9dd38fd" alt="Estado 3" width="300"/>
</p>

## Últimos 2 estados y estado inicial
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/0c6028d5-866c-4398-ba98-9b4ba156498c" alt="Estado 4" width="300"/>
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/1423f4ca-ae22-45df-b5a6-d53f7609bfce" alt="Estado 5" width="310"/>
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/aef1a94f-7b92-4a6d-a467-2ef14d1d3c3f" alt="Estado 6" width="300"/>
</p>
Como se puede observar en las anteriores imágenes, el sistema se comporta correctamente con los estados establecidos y la lógica encargada del funcionamiento del sistema corresponde con el planteamiento inicial de la solución. Por lo tanto, esta interfaz gráfica permite ver los estados del ciclo de trabajo del proceso automatizado de etiqueta de cajas de forma correcta, cumpliendo con los requerimientos de los entregables.

## Proceso de prototipado
Para diseñar el diagrama ladder para el funcionamiento del prototipo funcional, se realizo un diagrama de secuencia para tener una idea del proceso de acción del sistema automatizado. Así que se definió la siguiente tabla de variables:

<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/dee3087f-81eb-4c95-9d5f-65851f2739a8" alt="Inicial HMI" width="800"/>
</p>
Con las variables establecidas, se diseñó el siguiente diagrama de secuencia que refleja el funcionamiento del prototipo del sistema automático de etiquetado de cajas:

<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/1c2cf72d-36ae-41c7-a65e-2cced60264cb" alt="Inicial HMI" width="800"/>
</p>

Finalmente se diseño el siguiente diagrama Ladder para realizar el prototipo:
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/81392047/366d8db6-7626-4da9-86d4-2ee3ee6fdc5d" alt="Inicial HMI" width="800"/>
</p>

Para la parte física, se consulto un video que mencionaba la elaboración de una cinta transportadora casera a partir de un motor DC [6] junto con la base de la máquina etiquetadora automática mencionada al inicio del documento. Para el PLC se hizo uso de un microcontrolador esp32.
La creación del sistema en físico se desarrolló con materiales principalmente fáciles de obtener, como lo son el cartón industrial, cartón paja, silicona caliente para unir piezas, además de tubos PVC y palillos para realziar diferentes mecanismos giratorios. A lo largo de la creación se realizaron diferentes cambios debido a varios fallos repentinos, en primer lugar, los motores adquiridos no tenían la suficiente potencia para hacer girar la primera banda de la cinta transportadora, la cual se construyó en foamy negro, y es que su peso y fricción ocasionaban que el motor se detuviera por completo, para solucionar este problema se reemplazó por una banda realizada con tela, aunque esta vez ya no se detenía el motor, al agregar una caja sucedía de nuevo el problema anterior, por lo que se optó por cambiar al material más ligero que se encontró, una cinta de decoración plástica, su peso y su fricción eran adecuadas para permitir al motor seguir girando aún cuando habían hasta tres cajas a la vez en la banda.

<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/97065279/644cd0a3-2d4c-40ae-b2f9-f7eb18e4cc77" alt="Inicial HMI" width="500"/>
</p>

Además del problema con la banda giratoria, también nos enfrentamos a un desafío enorme y era la creación de la etiquetadora automático, y es que el material elegido para representar las etiquetas (papel fotográfico adhesivo) ocasionaba que el motor que accionaba dicho sistema no girara, ya que necesitaba más fuerza para poder enrollar un papel tan grueso, así que se hicieron estrategias manuales en las que se despegó la parte adhesiva de las zonas que iban pegadas a los rodillos giratorios, de modo se que se reduciera la fricción.
Lastimosamente el sistema no pudo ser probado en su totalidad de manera automatizada debido a que nos enfrentamos al problema de que los motores se consumían toda la corriente proveniente del Esp32, de modo que los otros componentes como los sensores dejaban de funcionar, se optó por utilización de baterías, lo cual en principio sería de ayuda para solucionar el error anteriormente dicho, sin embargo, aunque se trabajó arduamente para hacer funcionar el sistema, no se pudo encontrar solución al mayor de los problemas, y es que los motores con alimentación distinta al resto del sistema, no se activaban ni por los switches ni por los sensores.
<p align="center">
  <img src="https://github.com/PixelNote/Parcial-2-auto/assets/97065279/755bd388-288c-478b-bb13-c077bd09f69f" alt="Inicial HMI" width="500"/>
</p>

## Conclusiones
- El HMI es esencial para tener la retroalimentación acerca del estado del sistema. Se pueden conocer las variables, si se usan timer, se pueden conocer los tiempos entre procesos y también permite que el usuario no tenga que usar un ambiente físico, sino que puede interactuar con la interfaz para optimizar su proceso de logística y monitoreo de un proceso industrial.
- Aunque se tuvo un buen diseño y un buen HMI, en la práctica real, llevar a cabo sistemas que realizan trabajos naturalmente hechos por mano humana, se vuelve de labor intensiva y dificil de lograr a la perfección, y es que a diferencia de la mente humana, un sistema automatizado es más complejo de definir la manera de controlar que todo esté funcionando correctamente.

## Referencias

[1] “¿Qué es una cinta transportadora? Funcionamiento y aplicacio...” Core ceramics. Accedido el 1 de abril de 2024. [En línea]. Disponible: https://beltinglab.com/noticias/que-es-una-cinta-transportadora-funcionamiento-y-aplicaciones#:~:text=El%20funcionamiento%20de%20una%20cinta,accionados%20por%20un%20motor%20eléctrico.

[2] “Etiquetadora Triton-Júpiter”. Ferplast. Accedido el 2 de abril de 2024. [En línea]. Disponible: https://www.fer-plast.com/es/productos/embalaje/impresoras-y-etiquetado/dispensadores-de-etiquetas-y-etiquetadoras/etiquetadora-tritone-giove-detail

[3] “¿Qué es el diagrama escalera?”. Edrawsoft. Accedido el 6 de abril de 2024. [En línea]. Disponible: https://www.edrawsoft.com/es/article/what-is-ladder-diagram.html

[4] “Temporizador en TIA PORTAL”. Accedido el 6 de abril de 2024. [En línea]. Disponible: https://ingelearn.com/temporizadores-en-tia-portal-como-funcionan/#:~:text=Un%20temporizador%20TON,%20también%20llamado,cambia%20de%200%20a%201.

[5] Muy Fácil De Hacer. Proyectos | Cinta Transportadora Casera (muy fácil de hacer). (12 de marzo de 2016). Accedido el 6 de abril de 2024. [Video en línea]. Disponible: https://www.youtube.com/watch?v=7UsmJgHU6wk
