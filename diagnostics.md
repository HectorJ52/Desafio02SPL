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
