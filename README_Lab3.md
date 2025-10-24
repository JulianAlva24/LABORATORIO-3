# Laboratorio 3 - Configuración de Switch y Topologías de Red

**Autores:**  
- Daniel Felipe Pinilla Daza  
- Julián Sebastián Alvarado  

**Profesor:** Diego Alejandro Barragán  
**Universidad Santo Tomás – Sede Principal**  
**Bogotá D.C.**

---

## 📘 Introducción

En este laboratorio se desarrolló una práctica orientada a **conocer y configurar un switch gestionable** dentro de una red local (LAN).  
El propósito fue comprender el funcionamiento de los dispositivos de interconexión, su configuración mediante consola serial y la implementación de distintas **topologías de red** (estrella, árbol, anillo y parcial).

Durante el ejercicio, se accedió al sistema operativo del switch (IOS) usando **Putty** y el protocolo **RS-232**, explorando parámetros esenciales como:

- Memoria **NVRAM**  
- **Direcciones MAC** y **tabla ARP**  
- **Configuración de VLANs**  
- **Estado de interfaces**

Esto permitió fortalecer las competencias en administración de redes y comprender cómo los switches gestionan el tráfico, segmentan redes y optimizan la conectividad entre dispositivos como PC, Raspberry Pi y Arduino.

---

## 🎯 Objetivos

### Objetivo general
Implementar y analizar distintos métodos de **detección y control de errores en la comunicación serie entre microcontroladores**, comprendiendo su funcionamiento y efectividad.

### Objetivos específicos
1. Configurar comunicación UART entre Arduino y Raspberry Pi Pico.  
2. Implementar verificación mediante **checksum**.  
3. Desarrollar un protocolo de **retransmisión ACK/NACK**.  
4. Comparar las técnicas **VRC** y **LRC**.  
5. Documentar resultados destacando la importancia del control de errores.

---

## 🧠 Marco Teórico

### Switch
Dispositivo de capa 2 del modelo OSI que interconecta múltiples equipos en una LAN, gestionando el tráfico mediante **direcciones MAC**. Algunos switches también operan en capa 3, permitiendo la creación de **VLANs** y **rutas estáticas**.

### Memoria NVRAM
Almacena la configuración de inicio (*startup-config*) del switch, la cual permanece tras un reinicio.

### VLAN (Virtual Local Area Network)
Permiten dividir una red física en subredes lógicas, mejorando seguridad y administración.  
Ejemplo:
- VLAN 10 → Máscara /24 → 255.255.255.0  
- VLAN 20 → Máscara /16 → 255.255.0.0  
- VLAN 30 → Máscara /8 → 255.0.0.0  

### Puertos troncales y de acceso
- **Acceso:** conecta dispositivos finales.  
- **Troncal:** transporta múltiples VLANs entre switches o routers.

### Tabla ARP
Asocia direcciones IP con direcciones MAC para habilitar la comunicación dentro de la red.

---

## ⚙️ Desarrollo

### 🔹 Punto 1: Conociendo el Switch y Red Interna

**Objetivo:** Configurar un switch básico (24/48 puertos) disponible en la Universidad Santo Tomás.

**Materiales:**
- Cable consola (RJ-45 → DB9 o USB-Serial)  
- Software **Putty**  
- Cables Ethernet  
- 1 PC ETM y 1 portátil  

**Procedimiento:**
1. Conectar cable consola al puerto **CONSOLE** del switch.  
2. Configurar **Putty** en modo **Serial** (9600 baudios, 8N1, sin control de flujo).  
3. Acceder al IOS del switch y ejecutar comandos:
   - `show running-config` → Configuración actual en RAM  
   - `show startup-config` → Configuración almacenada en NVRAM  
   - `show version` → Ver modelo, número de serie y versión de IOS  
   - `show vlan brief` → VLANs existentes  
   - `show mac address-table` → Tabla MAC  
   - `show interfaces status` → Estado de interfaces

#### Creación de VLANs
En modo configuración:
```bash
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name RED1
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
```
De forma análoga se configuraron VLAN 20 (/16) y VLAN 30 (/8).

---

### 🔹 Punto 2: Conexiones Topológicas

Se desarrollaron y probaron distintas **topologías de red**:

#### 🔸 Topología en Estrella
- Todos los PCs conectados al switch.  
- Asignación manual de IPs a cada equipo.  
- Verificación mediante **ping** entre dispositivos.  
- El switch centralizó toda la comunicación exitosamente.

#### 🔸 Topología en Árbol
- El **PC1** actúa como raíz y los demás como ramas.  
- Al desconectar una rama, las otras continúan comunicando.  
- Se comprobó la robustez del árbol frente a fallos parciales.

#### 🔸 Topología en Anillo
- Los PCs se conectan en forma circular.  
- Al desconectar un nodo (PC2), toda la comunicación se interrumpe.  
- Se demuestra la vulnerabilidad de la topología anillo ante un único fallo.

#### 🔸 Topología Parcial
- Algunos equipos se comunican directamente entre sí sin pasar por el switch.  
- Aunque PC2 y PC3 estaban desconectados del switch, siguieron comunicándose entre ellos.  
- Esto demuestra flexibilidad en redes híbridas o parcialmente conectadas.

---

## 🧩 Resultados y Análisis

- Se logró establecer comunicación entre todos los equipos en las distintas topologías.  
- Las VLANs segmentaron correctamente el tráfico y permitieron el aislamiento lógico de subredes.  
- El uso de comandos IOS permitió observar y diagnosticar en tiempo real el comportamiento del switch.  

---

## 🧾 Conclusiones

1. La configuración de switches mediante CLI y herramientas como Putty permitió comprender el funcionamiento interno de los dispositivos de capa 2.  
2. Las VLANs demostraron su utilidad para **segmentar redes** y **mejorar seguridad** sin necesidad de hardware adicional.  
3. Las pruebas con diferentes topologías permitieron comparar su **tolerancia a fallos** y comportamiento de red, destacando que:
   - **Estrella y Árbol** son más estables ante fallos.  
   - **Anillo** es vulnerable a la desconexión de un nodo.  
   - **Parcial** permite comunicación flexible incluso con desconexiones del switch.

---

## 📚 Referencias

1. Cisco Systems. (2024). *Cisco IOS Configuration Fundamentals Command Reference.*  
2. Tanenbaum, A. S., & Wetherall, D. J. (2021). *Redes de computadoras* (6ª ed.). Pearson.  
3. Stallings, W. (2022). *Comunicaciones y redes de computadores* (10ª ed.). Pearson.  
4. Odom, W. (2020). *CCNA 200-301 Official Cert Guide Library.* Cisco Press.  
5. IEEE Standards Association. (2018). *IEEE 802.1Q - Virtual LANs Standard.*  
6. Kurose, J. F., & Ross, K. W. (2021). *Redes de computadoras: Un enfoque descendente* (8ª ed.). Pearson.  
7. Cisco Networking Academy. (2023). *Switching, Routing, and Wireless Essentials.*  
8. Forouzan, B. A. (2021). *Transmisión de datos y redes de comunicaciones* (5ª ed.). McGraw-Hill Education.

---

**© 2025 - Universidad Santo Tomás**  
_Laboratorio de Comunicaciones Industriales - Práctica 3_
