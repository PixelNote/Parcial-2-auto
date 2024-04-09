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
Cómo parte del proceso inicial, se consultaron algunas referencias para facilitar el proceso del diseño del prototipo funcional. Se tuvo la idea de realizar el diseño usando una cinta transportadora que siempre estaba en movimiento, ya que usualmente, las cadenas de suministro están en funcionamiento continuo [2]. Con la cinta transportadora siempre funcionando, la siguiente tarea era diseñar la lógica para el funcionamiento del sistema de etiqueta. 

Así que se buscaron algunas referencias de máquinas de etiqueta que existen en el mercado. Nos llamo la atención la máquina etiquetadora triton-júpiter ya que esta presenta el proceso de etiquetado de forma vertical [1]. Este sistema de etiquetado vertical favorecía el proceso con la cinta transportadora y su método de funcionamiento era simple para un futuro prototipado.

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
### 






## Referencias

[1] https://beltinglab.com/noticias/que-es-una-cinta-transportadora-funcionamiento-y-aplicaciones#:~:text=El%20funcionamiento%20de%20una%20cinta,accionados%20por%20un%20motor%20el%C3%A9ctrico.
[2] https://eurotransis.com/que-es-una-cinta-transportadora-principios-de-funcionamiento/
[3] https://www.edrawsoft.com/es/article/what-is-ladder-diagram.html
[4] https://ingelearn.com/temporizadores-en-tia-portal-como-funcionan/#:~:text=Un%20temporizador%20TON%2C%20tambi%C3%A9n%20llamado,cambia%20de%200%20a%201.
