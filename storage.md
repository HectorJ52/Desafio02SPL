# Almacenamiento - InnovaCloud Solutions

**Autor:** [William Aaron Peralta Cruz PC210574]
**Rol:** Consultor de Almacenamiento
**Fecha:** 22/08/2026

---

## 1. Análisis del Problema

Actualmente, el servidor principal de InnovaCloud Solutions presenta fallos de disco que han ocasionado pérdida de datos.
Esto es crítico porque:

La información de clientes y proyectos es el activo más valioso de la empresa.

Un solo disco dañado puede detener operaciones, afectar la disponibilidad de servicios y generar pérdidas económicas.

La infraestructura carece de redundancia, lo que significa que no existe un respaldo automático de los datos en caso de falla.

En términos simples: si un disco se rompe, la empresa se queda sin acceso a sus datos.


## 2. Propuesta de Solución: RAID

Implementar un sistema de RAID 1 (espejo) en los servidores principales.

¿Qué es RAID 1?
RAID significa Redundant Array of Independent Disks (Arreglo Redundante de Discos Independientes).

En RAID 1, los datos se escriben de forma idéntica en dos discos.

Si uno falla, el otro sigue funcionando con una copia exacta.

En palabras sencillas: cada archivo se guarda dos veces, en dos discos distintos, para que nunca se pierda.

Implementación Técnica con mdadm
1. Instalar la herramienta de administración
```sudo apt update```
```sudo apt install mdadm -y```

2. Crear el arreglo RAID 1
Suponiendo que los discos disponibles son /dev/sdb y /dev/sdc:
```sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc```

3. Verificar estado del arreglo
```cat /proc/mdstat```
```sudo mdadm --detail /dev/md0```

Guardar configuración permanente
```bash sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf```

5. Formatear y montar el RAID
```sudo mkfs.ext4 /dev/md0```
```sudo mkdir -p /mnt/raid1```
```sudo mount /dev/md0 /mnt/raid1```

6. Configurar montaje automático
Editar /etc/fstab y agregar:
```/dev/md0   /mnt/raid1   ext4   defaults   0   0```

---

## 3. Conclusión

Beneficios para InnovaCloud Solutions
Redundancia inmediata: los datos se mantienen disponibles incluso si un disco falla.

Continuidad del negocio: evita interrupciones críticas en los servicios.

Simplicidad: fácil de administrar y comprender, incluso para personal no técnico.

Escalabilidad futura: se puede evolucionar a RAID 10 para combinar redundancia y mayor rendimiento.
