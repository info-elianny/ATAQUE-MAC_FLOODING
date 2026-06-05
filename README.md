# 🛡️ Ataque MAC Flooding

📹 [Video demostración](https://youtu.be/lUHQbcc3u7w) | 📂 [Repositorio](https://github.com/info-elianny/ATAQUE-MAC_FLOODING)

---

## 📌 Objetivo del Laboratorio

Demostrar el funcionamiento de un ataque MAC Flooding en una red local, observando cómo el envío masivo de direcciones MAC falsas puede afectar la **tabla CAM** de un switch y alterar la forma en que este maneja el tráfico de red.

---

## 🎯 Objetivo del Script

Generar y enviar una gran cantidad de tramas Ethernet con **direcciones MAC de origen aleatorias** hacia un switch, con el propósito de llenar su tabla CAM y analizar el comportamiento del dispositivo cuando alcanza su capacidad máxima de almacenamiento.

---

## ⚙️ Requisitos

### Hardware y Software

- Un equipo que actuará como **atacante** (Kali Linux)
- Un **switch** administrable o no administrable susceptible a MAC Flooding
- Al menos un equipo conectado al switch para observar el comportamiento
- Todos los dispositivos en la **misma red local**
- **Python 3** instalado
- Biblioteca **Scapy** instalada
- Permisos de **administrador (root)** para ejecutar el script

### Instalación de dependencias

```bash
pip install scapy
```

---

## 🔧 Parámetros Utilizados

| Parámetro | Descripción |
|-----------|-------------|
| Interfaz de red | Adaptador desde el cual se realizará el ataque |
| Cantidad de tramas | Número de tramas Ethernet a enviar durante la prueba |
| Intervalo entre envíos | Tiempo en segundos entre cada trama enviada |
| MACs aleatorias | Generadas automáticamente para simular múltiples dispositivos |
| MAC de destino | Dirección MAC utilizada como destino de las tramas |
| Modo de ejecución | Cantidad específica de tramas o envío continuo hasta Ctrl+C |

---

## 🚀 Cómo Ejecutar el Script

```bash
sudo python3 mac_flooding.py
```

> ⚠️ **Debe ejecutarse con permisos root (sudo)**

---

## 📋 Funcionamiento del Script

**Paso 1:** El script comprueba que el usuario tenga permisos de administrador (root), ya que el envío de tramas Ethernet requiere privilegios elevados.

**Paso 2:** Se solicita la **interfaz de red**, la **velocidad de envío** y el **límite de tramas** a enviar.

**Paso 3:** El script genera **MACs aleatorias** y construye tramas Ethernet falsas para simular múltiples dispositivos conectados a la red.

**Paso 4:** Comienza a enviar continuamente las tramas con una MAC diferente en cada envío, llenando progresivamente la **tabla CAM** del switch.

**Paso 5:** Durante la ejecución, muestra en pantalla la cantidad de tramas enviadas y la velocidad de transmisión para monitorear el progreso.

**Paso 6:** El proceso continúa hasta alcanzar el límite configurado o hasta presionar **Ctrl+C**. Al finalizar, muestra un resumen con el total de tramas enviadas, el tiempo transcurrido y la velocidad promedio.

---

## 📸 Capturas de Pantalla

### Topología de Red
![Topología de Red](images/image2.png)

### Configuración de la red
![Configuración de la red](images/image3.png)

### Tabla CAM antes del ataque (MAC del router y Kali Linux)
![Tabla CAM antes del ataque](images/image4.png)

### Ataque corriendo
![Ataque corriendo](images/image5.png)

### Tabla CAM durante el ataque
![Tabla CAM durante el ataque](images/image6.png)

### Resultado en el switch
![Resultado en el switch](images/image7.png)

### Evidencia final
![Evidencia final](images/image8.png)

---

## 🌐 Documentación de la Red

| Dispositivo | Interfaz | Dirección IP | Máscara de Red |
|-------------|----------|--------------|----------------|
| R-1 | E0/0 | 6.6.1.1 | 255.255.255.0 |
| SW-1 | ---- | ----- | ----- |
| Kali Linux (Atacante) | e1 | 6.6.1.12 | 255.255.255.0 |
| Windows 7 (Víctima) | e0 | No se usó | No se usó |
| Cloud | net | 192.168.206.135 | 255.255.255.0 |

---

## 🛡️ Contramedidas para Mitigar el Ataque

### 1. Port Security
Limitar la cantidad de direcciones MAC que un puerto del switch puede aprender, evitando que la tabla CAM se llene con direcciones falsas. Cuando se supera el límite, el puerto se puede configurar para apagarse, restringirse o protegerse.

```
SW(config-if)# switchport port-security
SW(config-if)# switchport port-security maximum 2
SW(config-if)# switchport port-security violation shutdown
```

---

> ⚠️ **Este script es únicamente con fines educativos y de investigación en entornos controlados.**
