---
layout: post
title: Crear archivo para memoria SWAP
---

Vamos a ver como comprobar si existe un archivo de memoria SWAP y como crearlo.

* Comprobamos si el sistema tiene configurada la memoria SWAP:
```
sudo swapon --show
```
* Si no devuelve nada significa que no está configurada. Comprobamos la memoria libre:
```
free -h
```
* *Salida:*
```
Swap: 0B 0B 0B
```
* Comprobaos el espacio disponible en el disco:
```
df -h
```
* Creamos el archivo swap:
```
sudo fallocate -l 1G /swapfile
```
* Verificamos que se haya creado bien:
```
ls -lh /swapfile
```
* *Salida:*
```
-rw-r--r-- 1 root root 1,0G jun 11 11:39 /swapfile
```
* Lo hacemos accesible solo para root:
```
sudo chmod 600 /swapfile
```
* Comprobamos que se hayan asignado bien los permisos:
```
ls -lh /swapfile
```
* *Salida:*
```
-rw------- 1 root root 1,0G jun 11 11:39 /swapfile
```
* Asignamos el archivo como memoria SWAP:
```
sudo mkswap /swapfile
```
* *Salida:*
```
Setting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=6ef39959-1209-4bd9-a8f3-48bb19c193bb
```
* Habilitamos el archivo para memoria SWAP:
```
sudo swapon /swapfile
```
* Verificamos que esté habilitada:
```
sudo swapon --show
```
* Comprobamos la memoria libre:
```
free -h
```
* **Para hacer los cambios permanentes en el sistema:**
* * Hacemos un backup de /etc/fstab:
```
sudo cp /etc/fstab /etc/fstab.bak
```
* * Añadimos una línea:
```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
