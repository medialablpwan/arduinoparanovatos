<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/photo-1603732551658-5fabbafa84eb.jpg"/>
</div>
<br/>

<div align="justify">

# Arduino Course for Beginners

En esta página tomaré apuntes extraidos del curso “Arduino Course for Beginners”.

</div>

<div align="centre">

[![Arduino Course for Beginners](https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot%202023-12-17%20131855.png)](https://youtu.be/zJ-LqeX_fLU)

</div>

___

<div align="justify">

## CONCEPTOS BÁSICOS

En este apartado me centraré en describir los aspectos más básicos del hardware de la placa Arduino UNO R3, la más extendida. Echando un vistazo a la placa, se observa:

</div>

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-21_104445.png" align="right" width="500px"/>

- El LED “L” está conectado al pin 13, por lo que cada vez que se ponga a “high”, “L” se encenderá

- El chip principal es el MCU ATmega328

- Contamos con 14 pines digitales (no todos sirven para lo mismo y hay algunos especiales, como 6 capaces de PWM) y 6 pines analógicos

- LED RX y TX, que se encenderán cuando la placa esté recibiendo o enviando información, respectivamente

- Botón de reset que reinicia el código sin limpiar la memoria

<br clear="right"/>

<div align="justify">

___

## CONCEPTOS TÉCNICOS

En este apartado mostraré información técnica del microcontrolador [Arduino UNO](https://docs.arduino.cc/hardware/make-your-uno-kit), especialmente aspectos de la circuitería vistos en el grado.

### ESQUEMATICO

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182139.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

### PCB

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182120.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

### MODELO 3D

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182103.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

### PINOUT

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182305.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

</div>

___

<div align="justify">

## SOFTWARE DE INTERÉS

El software de interés para programar y testear código en Arduino se divide en IDEs y simuladores:

### IDES

Contamos con dos referentes:

- [Arduino IDE](https://www.arduino.cc/en/software): es el entorno de programación nativo de Arduino

- [PlatformIO](https://platformio.org/): es una extensión de Visual Studio Code que permite trabajar con las mismas funciones que en el IDE de Arduino, sumándole el mundo de las extensiones de VSC

### SIMULADORES

Es software que permite cargar los programas a una placa y componentes simulados que se comportarán de forma fiel a la realidad pudiendo así no comprometer el hardware real en la etapa de debugging:

- [TinkerCAD](https://www.tinkercad.com/): es una web que permite trabajar, de forma bastante limitada, con Arduino UNO y algunos componentes básicos

- [Fritzing](https://fritzing.org/): es un programa que funciona igual que TinkerCAD, con la diferencia de que expande mucho más la variedad de micros y componentes

___

<div align="justify">

## PROGRAMAS

En esta sección iré escribiendo programas para hacer que funcionen diversos proyectos con variedad de placas Arduino y componentes. Aplicaré los conocimientos de los apartados que iré desarrollando en los siguientes puntos.

### MI PRIMER PROGRAMA

<table>
<tr>
<th> Digital </th>
<th> Analogic </th>
</tr>
<tr>
<td>

```c++
// Declaración de variables
int pin_LED = 13;                         // Asigno al LED el pin 13
int valor_delay = 1000;                   // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	Serial.println("Inciando programa..."); // Mensaje por serial de inicio de programa
	pinMode (pin_LED, OUTPUT);              // Establecer 'LED_BUILTIN' como salida
}

// Código que se ejecuta infinitamente
void loop(){
	digitalWrite(pin_LED, HIGH);            // Encender el LED
	Serial.println("LED encendido");        // Notificación en el serial
	delay(valor_delay);                     // Durante 1 segundo
	digitalWrite(pin_LED, LOW);             // Apagar el LED
	Serial.println("LED apagado");
	delay(valor_delay);                     // Durante 1 segundo... Y vuelta a empezar
}
```

</td>
<td>

```c++
// Declaración de variables
int pin_LED = 9; // the PWM pin the LED is attached to
int brillo = 0; // Inicializo a '0' el brillo
int desvanecimiento = 5; // how many points to fade the LED by
int valor_delay = 1000;

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(9600);
	pinMode(pin_LED, OUTPUT);
}

// Código que se ejecuta infinitamente
void loop(){
   analogWrite(pin_LED, brillo); // A diferencia que 'digitalWrite', 'analogWrite' demanda el pin y el valor inicial de brillo en este caso
   brillo = brillo + desvanecimiento; // Cambiar la cantidad de brillo tras cada iteración de 'loop()'
   if (brillo == 0 || brillo == 255) { // Primera condición, 'brillo' va de 0 a 255 y, tras llegar a 255, va de 255 a 0
      desvanecimiento = -desvanecimiento; // Cambio de dirección en el extremo
   }
   delay(valor_delay); // Cada iteración de 'loop()' se mantiene por 300ms
}
```

</td>
</tr>
</table>

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-21_180848.png" width="400"  style="margin: 10px;"/>
	
  <em>Esquema del circuito para el programa “Hola mundo”</em>
</div>
<br/>

### SENSOR TEMPERATURA

En este programa se usa un sensor de temperatura TMP36 en el que, en tinkerCAD, yo le digo la temperatura que hace y me lo notifica por medio de unos LEDs configurados como salidas:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-22_184453.png" align="right" width="400px"/>

```c++
// Declaración de variables
#define LED_verde 6
#define LED_rojo 7
#define LED_azul 8
#define sensor_TMP A0

int mi_delay = 1000;

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	pinMode(LED_verde, OUTPUT);
	pinMode(LED_rojo, OUTPUT);
	pinMode(LED_azul, OUTPUT);
  pinMode(sensor_TMP, INPUT);
}

// Código que se ejecuta infinitamente
void loop(){
	int lectura = analogRead(sensor_TMP);
  Serial.print("La lectura analógica del sensor es: ");
	Serial.println(lectura);
	float vout = 5.0 / 1024 * lectura;                     // Lectura de 10bits transformada a voltaje de salida
	float temperatura = vout * 100 - 50;                   // La fórmula sería: T(ºC) = lectura(bits) * (5V / 1024bits) * (100ºC / 1V) - Tdesfase(ºC)
  Serial.print("La temperatura captada es: ");
	Serial.print(temperatura);
  Serial.print("\xC2\xB0");                              // Signo de "º"
  Serial.println("C");
  
	if(temperatura < 25 && temperatura > 15){
		digitalWrite(LED_verde, HIGH);
		delay(mi_delay);
		digitalWrite(LED_verde, LOW);
		delay(mi_delay);
	}
	else if(temperatura < 15){
		digitalWrite(LED_azul, HIGH);
		delay(mi_delay);
		digitalWrite(LED_azul, LOW);
		delay(mi_delay);
	}
	else{
		digitalWrite(LED_rojo, HIGH);
		delay(mi_delay);
		digitalWrite(LED_rojo, LOW);
		delay(mi_delay);
	}
}
```

<br clear="right"/>

### FUNCIONES LED

Función para que el usuario elija un color de LED y su frecuencia de parpadeo:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-30_180817.png" align="right" width="400px"/>

```c++
// Declaración de variables
int LED_azul = 6;
int LED_rojo = 7;
int LED_verde = 8;

// Declaración de funciones
void Parpadeo_LED(int pin, String color_LED, int valor_delay){
	digitalWrite(pin, HIGH);                                      // Encender el LED
	Serial.print("LED ");
	Serial.print(color_LED);                                      // Notificación en el serial
	Serial.println(" encendido");
	delay(valor_delay);                                           // Durante 1 segundo
	digitalWrite(pin, LOW);                                       // Apagar el LED
	Serial.print("LED ");
	Serial.print(color_LED);                                      // Notificación en el serial
	Serial.println(" apagado");
	delay(valor_delay);                                           // Durante 1 segundo... Y vuelta a empezar
}

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                                         // Inicializo el serial y sus baudios
	pinMode(LED_azul, OUTPUT);                                    // Establecer LEDs como salida
	pinMode(LED_rojo, OUTPUT);
	pinMode(LED_verde, OUTPUT);
}

// Código que se ejecuta infinitamente
void loop(){
	Parpadeo_LED(LED_azul, "azul", 1000);
	Parpadeo_LED(LED_rojo, "rojo", 2000);
	Parpadeo_LED(LED_verde, "verde", 3000);
}
```

<br clear="right"/>

### SENSOR DE HUMEDAD

Sensor de humedad que, por encima del 60%, nos notifica con el ‘BUILTIN_LED’ parpadeando:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-30_180612.png" align="right" width="400px"/>

```c++
// Declaración de variables
#define pin_sensor A0
#define mi_delay 1500

// Declaración de funciones
int Parpadeo_LED_BUILTIN(){
	digitalWrite(LED_BUILTIN, HIGH);
	delay(mi_delay);
	digitalWrite(LED_BUILTIN, LOW);
	delay(mi_delay);
}

bool Mi_clima_humedo(int humedad){
	if(humedad >= 65){
		Parpadeo_LED_BUILTIN();
		Serial.println("Estás en un clima humedo");
		return true;
	}
	else{
		Serial.println("Estás en un clima seco");
		return false;
	}
}

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                      // Inicializo el serial y sus baudios
	pinMode(pin_sensor, INPUT);
  pinMode(LED_BUILTIN, OUTPUT);
}

// Código que se ejecuta infinitamente
void loop(){
	float lectura = analogRead(pin_sensor);
	Serial.print("La lectura analógica es: ");
	Serial.println(lectura);
  float porcentaje_humedad = (lectura*100L)/876;
  Serial.print("La lectura del sensor de humedad es del ");
  Serial.print(porcentaje_humedad);
  Serial.println("%");
  Mi_clima_humedo(porcentaje_humedad);
  Serial.println();
	delay(mi_delay);
}
```

<br clear="right"/>

### PWM

Creo un algoritmo para variar de forma continua de menos a más, y viceversa, el duty cycle de la señal PWM generada en un pin digital. Para ello, uso la función ‘analogWrite(pin, valor)’ y bucles ‘for’. Además, controlo el brillo de forma analógica también de un LED:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-23_190211.png" align="right" width="400px"/>

```c++
// Declaración de variables
#define pin_LEDblanco 10
#define pin_LEDrojo 11
#define pin_analog A1
#define mi_delay 10

// Código que se ejecuta infinitamente
void setup()
{
	Serial.begin(115200);
	pinMode(pin_LEDblanco, OUTPUT);
	pinMode(pin_LEDrojo, OUTPUT);

}

// Código que se ejecuta infinitamente
void loop(){
	for(int desvanecer = 0; desvanecer <= 255; desvanecer++){
		analogWrite(pin_LEDblanco, desvanecer);
      	//Serial.println(desvanecer);
      	delay(mi_delay);
    }
  	
  for(int desvanecer = 255; desvanecer >= 0; desvanecer--){
  		analogWrite(pin_LEDblanco, desvanecer);
      	//Serial.println(desvanecer);
  		delay(mi_delay);
	}

	int lectura = analogRead(pin_analog);
	analogWrite(pin_LEDrojo, lectura / 4);
  	Serial.println(analogRead(pin_analog));
}
```

<br clear="right"/>

___

## VARIABLES GLOBALES Y LOCALES

La variables, dependiendo de dónde se definan, son globales o locales. Las globales se definen al principio del programa, antes del ‘setup()’ mientras que las locales se definen en cada función y son solo accesibles desde cada una de éstas.

```c++
// Declaración de variables
int variable_global = 5;                  // Variable gloabal 5 en todas las funciones de mi programa
int valor_delay = 1000;                   // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	int variable_local = 10;
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	Serial.println(variable_local);         // Imprime "10"

}

// Código que se ejecuta infinitamente
void loop(){
	int variable_local = 10;
	Serial.println(variable_local);         // Imprime "15" y sin sobreescribir 'variable_local' del 'setup()', ya que son locales y cada una es diferente
	delay(valor_delay);                     // Durante 1 segundo
}
```

___

</div>
