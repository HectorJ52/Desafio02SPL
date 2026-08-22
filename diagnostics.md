# Diagnóstico de Red - InnovaCloud Solutions

**Autor:** Geisel Gabriela Castellanos Flores | **Rol:** Consultora de Diagnóstico | **Fecha:** 22/08/2026

---

## 1. Análisis del Problema
La falta de un procedimiento estandarizado para auditar los servicios activos e interfaces en los servidores de InnovaCloud Solutions genera tiempos prolongados de inactividad, vulnerabilidades no detectadas y dificultad para aislar fallos de comunicación en la infraestructura corporativa.

---

## 2. Propuesta de Solución: Herramientas de Diagnóstico

### A. Verificación de Conectividad de Extremo a Extremo
* **Prueba de Conectividad Base (ICMP):**
  ```bash
  ping -c 4 192.168.1.1
Permite verificar la latencia y la pérdida de paquetes hacia una IP o puerta de enlace.

Trazado de Ruta:

Bash
traceroute 8.8.8.8
Identifica los saltos intermedios e interrupciones en la ruta de red.

B. Auditoría de Interfaces y Direccionamiento
Visualizar Interfaces Activas e IP:

Bash
ip a
Muestra el estado de enlace (UP/DOWN), dirección MAC e IP asignadas a cada tarjeta de red.

Tabla de Enrutamiento:

Bash
ip route
Valida la puerta de enlace predeterminada y las rutas locales.

C. Detección de Puertos Abiertos y Sockets
Listar Sockets y Puertos en Escucha:

Bash
ss -tulpn
Muestra puertos TCP (-t), UDP (-u), en escucha (-l), con procesos (-p) y en formato numérico (-n).

Escaneo de Red y Puertos (Nmap):

Bash
sudo nmap -sS -O 192.168.1.50
Identifica servicios activos y versión del sistema operativo en el servidor.

D. Auditoría y Control de Servicios en Ejecución
Estado de un Servicio Específico:

Bash
systemctl status nginx
Listar Servicios Activos:

Bash
systemctl list-units --type=service --state=running
3. Conclusión
La estandarización de estas herramientas de consola permite al equipo de TI identificar interrupciones con rapidez, auditar puertos vulnerables expuestos y garantizar la alta disponibilidad de los servicios críticos del cliente.
