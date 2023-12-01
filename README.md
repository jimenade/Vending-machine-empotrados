# Vending-machine-empotrados

Este proyecto consiste en crear una máquina espendedora, cuyo esquema de montaje es el siguiente:
![image](https://github.com/jimenade/Vending-machine-empotrados/assets/102520569/c6f9f506-be34-4232-95ed-3f220f9ce104)

El unico esquema que es un poco distinto es el del DHT11, porque no encontré uno de 3 patillas que el que tenemos nosotros y en este modelo el pin de señal y el de Vcc están intercambiados, así que está puesto tanto en el esquema como en el montaje original con mi señal y Vcc.

Primero en el void he creado un bucle para que el led parpadee, a la vez que se muestra en el LCD el mensaje "CARGANDO ...", una vez que el led ha parpadeado mostramos "servicio" en el LCD y pasamos a modo servicio.En este modo he creado una máquina de estados con 4 estados: 
## Estado 1:
Si no hay cliente a menos de 1m espera a que llegue un cliente.
Si hay cliente se pasa al estado b.

## Estado 2:
Se muestra la humedad y temperatura en el LCD durante 5 segundos. 
Una vez se muestre lo anterior se procede a mostrar en el LCD con los precios y se navega por el menú con el joystick y se selecciona con el switch del joystick y se pasa a mostrar en pantalla el mensaje "Preparando Cafe ..." y a la vez se ilumina un led de forma incremental mediante PWM, para hacer este proceso más realista se ejecuta esto último durante un tiempo aleatorio entre 4 y 8 segundos.
Después se muestra en el LCD durante 3 segundos "RETIRE BEBIDA" y se pasa al estado 1.

## Estado 3:
Este es el estado admin, al que se puede acceder si pulsas el botón durante más de 5 segundos, lo explico más adelante, se sabe que se está en este estado porque se encienden los 2 leds de forma simultánea.
En este estado se navega por un menú con 4 opciones que son: 
Ver la temperatura, Ver la distancia del sensor, ver el contador y modificar los precios.

## Estado 4:
Se muestra lo pedido en cada unode los casos por el LCD y se va actualizando mientras se va mostrando en el LCD.

# Interrupciones:
Si se pulsa el botón entre 2 y 3 segundos se vuelve al estado 1 y si se pulsa durante más de 5 segundos al estado 3. Para ello el botón tiene que estar en el pin 3 del arduino UNO que es uno de los pines designados para las interrpuciones y se calcula el tiempo cuando se cambia de alto a bajo y luego cuando pasa de bajo a alto, de ahí se hcae la resta y se calcula si pasar a un estado u otro.

# Problemas al hacer la práctica:
Para hacer las máquinas de estado y las selecciones de los menús estaba usando switch case, pero me di cuenta al incluir la interrupción que se liaba y tuve que cambiar a if else anidados.
Al principio tenía conectado el sensor de humedad al pin 1 y me daba valores muy raros y cambié el pin al 2 y empezó a dar valores normales.
Debido a lo que he mencionado anteriormente me faltaba un pin, así que el led que no uso PWM lo conecté a uno analogico y simulé un comportamiento digital poniendo 255 en vez de HIGH y 0 en vez de LOW.
El joystick en el centro da tanto valores altos como bajos, no se queda en un rango fijo de valores, así que a la hora de seleccionar alguna opción se complica un poco.

# Vídeos de demostración:

## Enseñando las interrupciones:
![Video 1](https://github.com/jimenade/Vending-machine-empotrados/assets/102520569/c9ce9641-6949-4432-a93a-05075f13fba8)

## Selección menú de admin:
![Video 2](https://github.com/jimenade/Vending-machine-empotrados/assets/102520569/cdd260a0-913c-4c5d-abfa-4c451f8b2693)

## Funcionamiento estados 1 y 2:
![Video 3](https://github.com/jimenade/Vending-machine-empotrados/assets/102520569/5ac233fa-373e-4fad-ac68-6a9d97ea5149)
