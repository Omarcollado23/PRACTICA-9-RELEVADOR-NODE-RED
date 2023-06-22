# RELAY CON NODE-RED (encender led)
Este repositorio muestra como podemos programar una ESP32 con SR- y ocupando la plataforma node-red y ahi poder la distancia y almismo tiempo graficar los resultados obtenidos.


### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un  (```RELAY```) para encender un LED por medio de un botón. Cabe aclarar que esta practica se usara en el simulador llamado [WOKWI](https://https://wokwi.com/). y ```localhost:1880```


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Relay (modulo)
- NODE-red (http://localhost:1880/#flow/214928d358894902)


## Instrucciones

### Requisitos previos

# Instalar Node-Red

## Pasos para instalación

1. Entrar a la pagina  https://nodejs.org/en
2. Descargar el archivo **18.16.0 LTS** como se muestra en la siguente imagen.

![](https://github.com/DiegoJm10/Node-red-instalcacion/blob/main/Node.js%20-%20Google%20Chrome%2014_06_2023%2005_04_00%20p.%20m..png?raw=true)

3. Abrir el archivo e instalar el programa [node.js](https://nodejs.org/en)


4. Nos vamos al inicio y buscamos ```cmd``` cOmo se muestra en la imagen y damos clic en "ejecutar como administrador" y escribir lo siguente:
![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/CMD.png?raw=true)

```
npm install -g --unsafe-perm node-red
```
![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/npm%20instal.png?raw=true)

5. Despues comprobamos que funcione node-red con el siguente codigo: (con este mismo codigo podemos arrancar el programa siempre que lo necesitemos)

```
node-red
```
 ![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/node-red.png?raw=true)


 ## Arranque de programa

Para abrir la aplicación nos vamos algun explorador y colocamos el siguente link:    ```localhost:1880```


## Instalación de Dashboard

1. Abrimos la pestaña de opciones y elegimos ```Manage palette``` 

![](https://github.com/DiegoJm10/Node-red-instalcacion/blob/main/Node.js%20-%20Google%20Chrome%2014_06_2023%2005_06_26%20p.%20m..png?raw=true)

2. Seleccionamos **Install* y buscamos ```node-red-dashboard```.
3. Seleccionamos ```node-red-dashboard```.
![](https://github.com/DiegoJm10/Node-red-instalcacion/blob/main/Node.js%20-%20Google%20Chrome%2014_06_2023%2005_06_17%20p.%20m..png?raw=true)


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "Alexcollado";
const char* mqttPassword = "190323";
const char* mqttTopic = "OmarDAI&M";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}
```
2. Instalamos la libreria de  **ArdiunoJson** y **PubSubClient** como se muestra en la siguente imagen, dando clic en (Library Manager) y despues en el simbolo de (+)

![]()

3. Hacemos las conexiones el **ESP32** con el RELAY como se muestra en la siguiente imagen:

![]()

### Instrucciónes de operación

1. Iniciamos el simulador.
2. Visualizar los datos en el monitor serial.
3. Nos dirigimos al dashboard para encenrder con el switch


# Instrucciones para hacer la conexión con NODE-RED

Abrimos una nueva pestaña en el navegador que utilizas e insertamos en la barra de navegación el suiguiente link (localhost:1880)

![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/localhost1.png?raw=true)

1. Colocamos bloque ```switch```. y configuramos así como se muestra en la imagen.

![]()

Colocamos bloque ```mqqtt out```.
2. Configurar el bloque con el puerto mqtt con el IP ```44.195.202.69:1883``` como se muestra en la imagen.

![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/%23servidor.png?raw=true)

3. Colocar el bloque json y configurarlo como se muestra en la imagen.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/JSON.png?raw=true)

Colocamos tres bloques function y lo configuramos con el siguente codigo.

```
msg.payload = msg.payload.TEMPERATURA;
msg.topic = "TEMPERATURA";
return msg;
```

![](https://github.com/Omarcollado23/PRACTICA-6-DHT22-CON-NODE-RED/blob/main/conf%20temp.png?raw=true)


```
msg.payload = msg.payload.HUMEDAD;
msg.topic = "HUMEDAD";
return msg;
```

![](https://github.com/Omarcollado23/PRACTICA-6-DHT22-CON-NODE-RED/blob/main/conf%20hum.png?raw=true)

```
msg.payload = msg.payload.DISTANCIA;
msg.topic = "DISTANCIA";
return msg;
```
![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/code%20distancia.png?raw=true)

5. Colocamos los bloques de chart y gauge.
Los configuramos de la siguiente manera como se muestra en las siguientes imagenes.

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/conf%20temp.png?raw=true)

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/conf%20hum.png?raw=true)

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/conf%20dist.png?raw=true)

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/graficos.png?raw=true)


## Resultados

Cuando haya funcionado, la información obtenida del sensor **DHT22** Y **HC-SR04** los mandara a servidor cuando se haga conexión y los observaremos por medio del dashboard. 

Al final el diagrama nos queda de la siguiente manera:

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/esquema.png?raw=true)

Despues damos Clic en el boton donde dice ```Deploy``` para cargar el programa y despues oprimos la pestaña con una flechita señalnado hacia arriba en diagonal, como se muestra en la imagen. 

![](https://github.com/Omarcollado23/PRACTICA-7-CON-ULTRASONICO/blob/main/deploy.png?raw=true)

Y veremos los resultados que manda el sensor al servidor como se muestra en la imagen.

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/result%201.png?raw=true)

![](https://github.com/Omarcollado23/PRACTICA-8-DHT22-CON-ULTRASONICO/blob/main/result2.png?raw=true)
![]()
![]()
![]()



# Créditos

Desarrollado por Ing. Omar Alejandro Collado Carriola

- [GitHub](https://github.com/Omarcollado23)