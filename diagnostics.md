# Diagnóstico de Red - InnovaCloud Solutions

**Autor:** Geisel Gabriela Castellanos Flores
**Rol:** Consultora de Diagnóstico
**Fecha:** 22/08/2026

---

## 1. Análisis del Problema

La falta de un procedimiento estandarizado para auditar los servicios activos e interfaces en los servidores de **InnovaCloud Solutions** genera tiempos prolongados de inactividad, vulnerabilidades no detectadas y dificultad para aislar fallos de comunicación en la infraestructura corporativa.

---

## 2. Propuesta de Solución: Herramientas de Diagnóstico

### Paso 1: Prueba de Conectividad Base (ICMP)

El comando `ping` permite comprobar la conectividad entre el servidor y otro dispositivo de la red.

```bash
ping -c 4 192.168.1.1
```

**Descripción:**
Envía 4 paquetes ICMP a la dirección IP `192.168.1.1` para verificar si existe conectividad y medir posibles pérdidas de paquetes y tiempos de respuesta.

---

### Paso 2: Trazado de Ruta

El comando `traceroute` permite identificar los diferentes saltos que atraviesa un paquete hasta llegar a su destino.

```bash
traceroute 8.8.8.8
```

**Descripción:**
Identifica los saltos intermedios e interrupciones en la ruta de red, ayudando a localizar posibles problemas de comunicación.

---

### Paso 3: Visualizar Interfaces Activas e IP

El comando `ip a` permite consultar las interfaces de red disponibles en el servidor.

```bash
ip a
```

**Descripción:**
Muestra el estado de enlace (`UP/DOWN`), dirección MAC e IP asignadas a cada tarjeta de red.

---

### Paso 4: Validar Tabla de Enrutamiento

El comando `ip route` permite consultar las rutas configuradas en el sistema.

```bash
ip route
```

**Descripción:**
Valida la puerta de enlace predeterminada y las rutas locales utilizadas por el servidor para comunicarse con otras redes.

---

### Paso 5: Listar Sockets y Puertos en Escucha

El comando `ss` permite consultar las conexiones de red y los puertos que se encuentran activos.

```bash
ss -tulpn
```

**Descripción:**
Muestra:

* `-t`: conexiones TCP.
* `-u`: conexiones UDP.
* `-l`: puertos en escucha.
* `-p`: procesos asociados.
* `-n`: direcciones y puertos en formato numérico.

---

### Paso 6: Escaneo de Red y Puertos (Nmap)

Nmap permite identificar puertos, servicios y características del sistema remoto.

```bash
sudo nmap -sS -O 192.168.1.50
```

**Descripción:**
Realiza un escaneo TCP SYN y trata de identificar el sistema operativo del servidor. Permite detectar servicios activos y posibles puertos expuestos.

> **Nota:** El uso de Nmap debe realizarse únicamente sobre equipos y redes para los cuales se tenga autorización.

---

### Paso 7: Estado de un Servicio Específico

El comando `systemctl status` permite consultar el estado de un servicio administrado por `systemd`.

```bash
systemctl status nginx
```

**Descripción:**
Muestra el estado actual del servicio web **Nginx**, incluyendo si está activo, detenido o presenta algún error.

---

### Paso 8: Listar Servicios Activos

El siguiente comando permite visualizar los servicios que se encuentran actualmente en ejecución.

```bash
systemctl list-units --type=service --state=running
```

**Descripción:**
Lista todos los servicios del sistema que se encuentran actualmente activos y en ejecución.

---

## 3. Conclusión

La estandarización de estas herramientas de consola permite al equipo de TI identificar interrupciones con mayor rapidez, auditar puertos vulnerables expuestos y garantizar la alta disponibilidad de los servicios críticos de **InnovaCloud Solutions**.

El procedimiento propuesto facilita un diagnóstico ordenado de la infraestructura, comenzando por la comprobación de conectividad y continuando con el análisis de rutas, interfaces, puertos, servicios y procesos activos.
