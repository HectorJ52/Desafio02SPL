# Configuración de Red

**Autor:** [Jefferson Ulises Martínez López]

---

## 1. Problema: Limitaciones del modo NAT en VirtualBox

InnovaCloud Solutions utiliza actualmente el modo de red **NAT (Network Address Translation)** por defecto en sus máquinas virtuales de VirtualBox. Aunque este modo es útil para salir a internet de forma rápida, presenta varias limitaciones críticas en un entorno de desarrollo colaborativo:

- **Aislamiento entre máquinas virtuales:** cada VM vive detrás de su propia NAT, por lo que dos máquinas virtuales de la misma empresa no pueden verse ni comunicarse entre sí de forma nativa, aunque estén corriendo en el mismo host.
- **Sin acceso desde la red corporativa:** los equipos físicos de la red interna de InnovaCloud no pueden alcanzar servicios corriendo dentro de las VMs (por ejemplo, un servidor web de pruebas), porque el NAT solo permite tráfico saliente desde la VM hacia afuera, no entrante desde la LAN.
- **IP no persistente ni predecible:** la IP asignada por NAT es interna del hipervisor y cambia según la configuración de cada host, lo que complica automatizar despliegues, scripts o inventarios de servidores.
- **Redirección de puertos manual y limitada:** para exponer un servicio hacia afuera hay que configurar port forwarding VM por VM, lo cual no escala cuando se trabaja con varios servidores de prueba en paralelo.

En resumen, NAT funciona bien para un usuario aislado que solo necesita internet, pero es un obstáculo real cuando varios desarrolladores necesitan que sus máquinas virtuales se comuniquen entre sí y con el resto de la infraestructura del cliente.

---

## 2. Comparación de modos de red en VirtualBox

| Modo | Comunicación VM-VM | Comunicación VM-Red física | Acceso a Internet | Caso de uso típico |
|---|---|---|---|---|
| **NAT** | No (cada VM aislada) | No | Sí | Uso individual, navegación rápida, sin necesidad de exponer servicios |
| **Puente (Bridged)** | Sí | Sí, la VM aparece como un equipo más en la LAN | Sí | Servidores que deben integrarse a la red corporativa como si fueran equipos físicos |
| **Red Interna (Internal Network)** | Sí, entre VMs del mismo "switch virtual" | No (aislada del host y de la LAN) | No | Laboratorios cerrados, pruebas de seguridad, entornos aislados |

**Puente:** la VM obtiene una IP dentro del mismo segmento de red que el host físico (normalmente vía DHCP del router corporativo), como si fuera un dispositivo más conectado al switch. Ideal cuando el servidor necesita ser visible para toda la red.

**NAT:** la VM queda detrás de una capa de traducción de direcciones controlada por VirtualBox. Solo puede iniciar conexiones hacia afuera, nunca recibir conexiones entrantes sin redirección de puertos.

**Red Interna:** crea una red privada virtual que solo existe entre las VMs configuradas con el mismo nombre de red interna. No hay salida a Internet ni contacto con el host, lo que la hace perfecta para pruebas controladas pero inútil para desarrollo colaborativo con acceso externo.

---

## 3. Propuesta: modo recomendado para InnovaCloud Solutions

Se recomienda el modo **Puente (Bridged Adapter)** para la red de desarrollo de InnovaCloud Solutions.

**Justificación:**

- Cada máquina virtual obtiene una IP real dentro de la red corporativa, permitiendo que los desarrolladores se comuniquen entre sus VMs sin configuración adicional.
- Los servidores de prueba quedan accesibles desde cualquier equipo de la red interna, facilitando pruebas de integración entre distintos servicios (por ejemplo, un servidor web consultando una base de datos en otra VM).
- No requiere redirección manual de puertos como en NAT, lo que reduce el trabajo operativo y el margen de error.
- Al asignarse una IP estática (ver sección 4), los servidores se vuelven predecibles y fáciles de referenciar en documentación, scripts de despliegue o monitoreo.

El modo Red Interna se descarta porque aísla completamente del resto de la red, y NAT se descarta porque impide la comunicación directa entre VMs, que es justo el problema que se busca resolver.

---

## 4. Configuración de IP estática con Netplan

Una vez seleccionado el modo Puente, se configura una IP estática en cada máquina virtual Ubuntu Server para que su dirección no cambie entre reinicios.

### Paso 1: Identificar la interfaz de red

```bash
ip a
```

Esto muestra las interfaces disponibles. En Ubuntu Server suele llamarse algo como `enp0s3` o `eth0`.

### Paso 2: Ubicar el archivo de configuración de Netplan

```bash
ls /etc/netplan/
```

Netplan normalmente trae un archivo `.yaml` por defecto, por ejemplo `00-installer-config.yaml` o `50-cloud-init.yaml`, dependiendo de cómo se instaló el sistema.

### Paso 3: Editar el archivo YAML

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Contenido de ejemplo para una IP estática:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.1.50/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

**Nota:** los valores de `addresses`, `via` y el nombre de la interfaz deben ajustarse al rango real de la red de InnovaCloud Solutions.

### Paso 4: Aplicar la configuración

```bash
sudo netplan apply
```

### Paso 5: Verificar los cambios

```bash
ip a
ping -c 4 8.8.8.8
```

Con esto se confirma que la interfaz tomó la IP estática asignada y que hay salida a Internet a través de la red corporativa.

---

## Conclusión

Cambiar de NAT a Puente, junto con una configuración de IP estática mediante Netplan, resuelve el problema de comunicación entre máquinas virtuales y con la red corporativa de InnovaCloud Solutions, dejando los servidores de desarrollo accesibles, predecibles y fáciles de administrar.
