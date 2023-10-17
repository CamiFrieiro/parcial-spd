Alumno
----
CAMILA FRIEIRO - 1B

Parte 1: Contador y 7 segmentos
----
[![](https://i.ibb.co/826zXbR/contador.png)](https://github.com/CamiFrieiro/parcial-spd/blob/main/Img/contador.png)

Descripcion
----
Es un contador del rango 0 a 99, utilizando 2 displays de 7 segmentos y 3 pulsadores para controlarlo.
Contara con un:
+ Pulsador que iniciara el contador desde 00 en forma ascendente
+ Pulsador que disminuira el contador
+ Pulsador que reseteara desde 00

Funciones
----
**PRENDER DIGITOS**

Esta funcion permitira el control de los displays. 
Tomamos 'digito' que en este caso seria UNIDAD (el display que nos permitira visualizar los numeros en unidad, los del display derecho) y DECENA (los numeros del display izquierdo)


```cpp
void prendeDigito(int digito) {
  if (digito == UNIDAD) {
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
    delay(timeDisplayON);
  } else if (digito == DECENA) {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
    delay(timeDisplayON);
  } else {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
}
```
Si digito es igual a "UNIDAD", entonces la función apagara el dígito de las unidades y encendera el dígito de las decenas.
Algo parecido pasara con la logica en "DECENA" y "RESET" (el pulsador numero 3)



**PRINT COUNT**

Anteriormente se ha definido una funcion donde se define cada numero del 0 al 9 con los segmentos.
Esta funcion es para mostrar esos numero en los displays.

```cpp
void printContador(int contar) {  
  int decenas = contar / 10;
  int unidades = contar % 10;

  prendeDigito(DECENA);
  printDigit(decenas);

  prendeDigito(UNIDAD);
  printDigit(unidades);
}
```
Para saber en cual de los dos displays corresponde creamos la variable "contar" donde realizamos un calculo para determinar unidad o decena.
Despues de se llama a la funcion printDigit (donde estan definidos los segmentos)

**KEYPRESSED**

Esta funcion esta relacionado con el manejo de los pulsadores

```cpp

int keypressed() { 
  sube = digitalRead(SUBE);
  baja = digitalRead(BAJA);
  reset = digitalRead(RESET);

  if (sube == 1)
    subePrevia = 1;
  if (baja == 1)
    bajaPrevia = 1;
  if (reset == 1)
    resetPrevia = 1;

  if (sube == 0 && sube != subePrevia) {
    subePrevia = sube;
    return SUBE;
  }
  if (baja == 0 && baja != bajaPrevia) {
    bajaPrevia = baja;
    return BAJA;
  }
  if (reset == 0 && reset != resetPrevia) {
    resetPrevia = reset;
    return RESET;
  }

  return 0;
}

```
Los pulsadores que hemos definido "BAJA", "SUBE", "RESET"correspondientes a su tarea.
La funcion permite detectar cual de ellos ha sido pulsado (si no fue pulsado = 0 y si lo fue = 1) y devuelve un valor dependiendo del estado.


**LOOP**

Ya creadas las funciones las implementamos en el loop:

```cpp
void loop() {
  int pressed = keypressed();
  if (pressed == SUBE) {
    contador++;
    if (contador > 99) {
      contador = 0;
    }
  } else if (pressed == BAJA) {
    contador--;
    if (contador < 0) {
      contador = 99;
    }
  } else if (pressed == RESET) {
    contador = 0;
  }

  printContador(contador);
}
```

Llamamos a la funcion keypressed para saber el estado de cada pulsador y los compara:
+ Si (if) el pulsador esta en estado "SUBE" el contador sera ascendete.
+ Si (if) el pulsador esta en estado "BAJA" el contador sera descendete.
+ Si (if) el pulsador esta en estado "RESET" el contador volvera a 0.

La multiplexacion fue utilizada para que ambos displays pueden prenderse gracias a dos funciones principales: printContador y prendeDigito
La primera designa Unidad y Decena mientras que la segunda prende un display y apaga otro de una manera consecutiva y rapida lo que crea la ilusion de que ambos estan prendidos casi al mismo tiempo.

Link Tinkercad
----
[Parte 1](https://www.tinkercad.com/things/7J4F92JR6hc-proyecto/editel?sharecode=Fxnb-mVDTuvV-FXFFJfxPC3JdhL5-eoPPOkeag03rlw "Parte 1")

Link Video
----
[Clase 04 - SPD](https://www.youtube.com/watch?v=_Ry7mtURGDE&t=77s "Clase 04 - SPD")


#Parte 2: Slide Switch, Motor y Sensor
----
[![](https://i.ibb.co/9b9QT7C/motor-y-sensor.png)](https://github.com/CamiFrieiro/parcial-spd/blob/main/Img/motor_y_sensor.png)

Descripcion
----
En esta parte sustituiremos los pulsadores del ejercicio anterior por un interrumptor deslizante (slide switch) de dos posiciones.
Dependiendo de la posicion se mostrara el contador normal de 0 a 99 y en la otra solo los numeros primos en ese mismo rango.

Tambien al proyecto se le agrego un Motor CC y un Sensor de Temperatura, donde el motor se prendera cuando el sensor de temperatura detecte una temperatura mayor o igual a 40°C.

Componentes agregados
----
**MOTOR CC**

El motor cc o motor de corriente continua es un componente que convierte la energía electrica en movimiento mecanico.
Es una maquina que utiliza electricidad para girar y realizar trabajos mecanicos . El motor necesitara un suministro estable de alimentacion y debera asegurarse de que tanto el cableado como la tension de entrada son los correctos.

**SENSOR DE TEMPERATURA**

Es un dispositivo que se usa para medir y detectar la temperatura de un entorno u objeto.
Captura cambios en propiedades fisicas o electricas cuando la temperatura tiene alguna variacion, estos cambios los sensores los convierten en una señal la cual puede ser interpretada por un sistema electronico.
Se utiliza en cosas rutinarias como lo son: los termostatos en casas y son esenciales para medir la temperatura.

Funciones Agregadas
----
**ES PRIMO**

Funcion que determina si un numero es primo o no

```cpp
bool esPrimo(int numero) {
  if (numero <= 1) return false;
  if (numero <= 3) return true;
  if (numero % 2 == 0 || numero % 3 == 0) return false;
  for (int i = 5; i * i <= numero; i += 6) {
    if (numero % i == 0 || numero % (i + 2) == 0) return false;
  }
  return true;
}

  return 0;
}
```
La funcion devuelve un valor booleano verdadero o falso dependiendo si es un numero primo o no.
+ Si el numero es 2 o 3, devuelve verdadero
+ Si el numero es divisible por 2 o 3, es falso 
+ Se realiza un bucle for para verificar si es divible por otros numeros mayores. Si el numero es divisible por "i" o por "i + 2" es falso
+ Si ninguna de las anterioes ocurre es verdadero

**CONTROLAR EL MOTOR**

Funcion para apagar o prender el motor cc

```cpp
void controlarMotor(float temperatura) {
  if (temperatura < 184) {
    digitalWrite(motor, LOW);
  } else {
    digitalWrite(motor, HIGH);
  }
}
```
Basicamente se realiza un if donde como condicion se determina que el motor se mantendra apaga si la temperatura es menor a 184 (lo que equivale a alrededor de 40°C y si esta es mayor entonces se encedera.

**LOOP**

```cpp
void loop() {
  interruptor = digitalRead(INTERRUPTOR_DESLIZANTE);
  temperatura = analogRead(sensor);

  if (interruptor == 0) {
    interruptorEncendido = false;
    unsigned long tiempoActual = millis();
    if (tiempoActual - ultimoTiempoDisplay >= intervaloDisplay) {
      ultimoTiempoDisplay = tiempoActual;
      do {
        numeroPrimoActual++;
        if (numeroPrimoActual > 99) {
          numeroPrimoActual = 0;
        }
      } while (!esPrimo(numeroPrimoActual));
      imprimirContador(numeroPrimoActual);
    }
  } else {
    interruptorEncendido = true;
    unsigned long tiempoActual = millis();
    if (tiempoActual - ultimoTiempoDisplay >= intervaloDisplay) {
      ultimoTiempoDisplay = tiempoActual;
      contador++;
      if (contador > 99) {
        contador = 0;
      }
      imprimirContador(contador);
    }
  }
  controlarMotor(temperatura);
}
```
En el loop inicializamos el interruptor y el sensor.
Se lee el estado del interrumptor, si este se encuentra en OFF (0) entonces devolvera false lo cual significa que en los displays se mostraran los numeros primos
Si esta en ON (1) devolvera verdadero por lo que el display mostrara el contador de 0 a 99 normal
Por ultimo se muestra la funcion controlarMotor que permitira que el motor se prende o apague segun sus condiciones

Link Tinkercad
----
[Parte 2](https://www.tinkercad.com/things/7MkaZfAyBzZ-copy-of-proyecto-parte-2-con-primos/editel?sharecode=knRkcRk3KNB-DPuPsjtqXfAlPJsncTxqT-ERfmV_QTo "Parte 2")

Link Bibliografia
----
[Motor CC ](https://www.linak-latinamerica.com/segmentos/techline/actuator-academyindustrial-actuators/c%C3%B3mo-funciona-un-motor-de-cc-en-un-actuador-industrial/#:~:text=Un%20motor%20de%20corriente%20continua,vez%2C%20crea%20un%20movimiento%20giratorio. "Motor CC ")
[Motor CC](https://sdindustrial.com.mx/blog/motor-de-corriente-continua/ "Motor CC")
[Sensor de Temperatura](https://proyectoarduino.com/sensor-de-temperatura/ "Sensor de Temperatura")
[Motor y Sensor](https://www.youtube.com/watch?v=MoHDZ5agMY4&t=370s "Motor y Sensor")

#Parte 3: Sensor de Luz ambiental
----
[![](https://i.ibb.co/Fn0cSsQ/ambiental.jpg)](https://github.com/CamiFrieiro/parcial-spd/blob/main/Img/ambiental.jpg)

Descripcion
----
En esta parte, dependiendo del numero de documento se agregara un componente al proyecto.
Por lo tanto me toco un: **Sensor de luz ambiental**

----
**SENSOR DE LUZ AMBIENTAL**

El sensor de luz ambiental es un componente electronico que es utilizado para detectar la luz del entorno que rodea al proyecto, cual es la intensidad de la luz. Su funcion es convertir esa intencidad en una señal electrica que pueda ser procesada por el arduino.

Se la suele utilizar para cosas como controlar la iluminacion o ahorrar energia.

**SENSOR DE TEMPERATURA**

Es un dispositivo que se usa para medir y detectar la temperatura de un entorno u objeto.
Captura cambios en propiedades fisicas o electricas cuando la temperatura tiene alguna variacion, estos cambios los sensores los convierten en una señal la cual puede ser interpretada por un sistema electronico.
Se utiliza en cosas rutinarias como lo son: los termostatos en casas y son esenciales para medir la temperatura.

Fue agregado en:
----
**CONTROLAR EL MOTOR**

El sensor fue agregado como una condicion en la funcion perteneciente al encendido del motor:

```cpp
void controlarMotor(float temperatura, int luzAmbiental) {
  if (temperatura < 184 && luzAmbiental < 800) {
    digitalWrite(motor, HIGH);
  } else {
    digitalWrite(motor, LOW);
  }
}
```
Despues de haber conectado el sensor (incluida una resistencia para el mismo), se lo inicializo al comienzo del codigo y en el setup.
Luego, para encencer el motor ahora no solo se necesitara que la temperatura sea menos a 184 (equivalente a 40 °C) sino que tambien la lectura del sensor de luz ambiental tiene que ser menor a 800.

Cumplidas ambas condiciones el motor se encendera.

Link Tinkercad
----
[Parte 3](https://www.tinkercad.com/things/dGwKWvHCBju-proyecto-parte-3/editel?sharecode=6zIIOl8pNfD8fH7yezvE-XdlrjnPV-MW09F3YUCFa9Q "Parte 3")

Link Bibliografia
----
[Sensor de luz ambiental](https://www.youtube.com/watch?v=75QtrOT3Ig0 "Sensor de luz ambiental")

[Sensor de luz ambiental](https://www.youtube.com/watch?v=kv6r6HzJDqw&t=43s "Sensor de luz ambiental")

Observaciones
----
A la hora de realizar la funcion de buscar el numero primo, intente hacerla de diferentes formas que al comienzo funcionaban pero al integrarlas al loop con el interrumptor no lo hacian.
Ademas de que tambien al agregar esta funcion la multiplexacion se rompia y tantos los numeros primos como el contador en el display no coordinaban, esto parece haberse solucionado cuando implemente millis() a los numeros primos, pero la solucion no parece 100% efectiva ya que hay un delay entre displays mas notable que en la parte 1.

En cuanto al sensor ambiental, originalmente queria integrarlo en relacion a los displays y las luces de los segmentos ya sea apagandolos o controlando la intensidad de ellas.
Si bien pude hacer que esto funcionara de manera separada, al querer integrarlo al loop perteneciente al codigo no logre que funcionara y no pude encontrar el error porque en el serial.print parecia estar todo correcto.
