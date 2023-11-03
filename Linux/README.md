# CheatSheets Linux

## Table of Contents
- [Start linux httpd](#start-linux-httpd)
- [Commands unix](#commands-unix)

## Start linux httpd
To start the server, as root type:
```bash
service httpd start
```

## Commands unix
```bash
pwd       -> para saber en que carpeta estas parado
ls        -> lista los archivos
ls -l     -> lista los archivos y muestra info de detalle
ls -lt    -> lista los archivos y los ordena de mas reciente a menos.
ls -lrt   -> lista los archivos y los ordena de mas menos a mas reciente.
cd        -> para ir a otra carpeta
scp       -> permite realizar la transferencia de archivos (cuando se salta a un servidor y se quiere copiar un archivo al server desde donde saltamos)
find -size +100M -printf "%s\t%p\n"     -> tamaño archivos en los folders y subfolders
df -k     -> para conocer el espacio disponible por carpeta en el raiz
/usr/bin/df -k -> para conocer el espacio disponible por carpeta en el raiz
tail -1000 system.log    -> para visualizar las ultimas 1000 lineas del archivo system.log en el directorio donde se encuentra ubicado.

pbrun -h wasapplicationbeta01 pbksh <<< "cd /applications/app1/logswas; tail -500 myapp.log"
pbrun -h wasapplicationsrv07 pbksh <<< "cd /applications/app1/logs; grep 'ANY_TEXT_SEARCH' system.log"
pbrun -h wasapplicationsrv07 pbksh <<< "cd /applications/app1/logs; scp system.log"
```