[![en](https://img.shields.io/badge/lang-en-red.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DataBase/Redis/README.md) [![es](https://img.shields.io/badge/lang-es-yellow.svg):ballot_box_with_check:](#)

# CheatSheets Redis

## Tabla de Contenido
- [¿Qué es Redis?](#¿qué-es-redis)
- [¿Qué es una caché Distribuida?](#¿qué-es-una-caché-distribuida)
- [Ventajas de la caché](#ventajas-de-la-caché)
- [Redis Sentinel vs Cluster](#redis-sentinel-vs-cluster)

## ¿Qué es Redis?
Es un motor de base de datos, empleado usualmente como una base de datos en memoria que funciona bajo el esquema de key-value(clave-valor), puede ser utilizado como caché, cola de mensajes, administrador de sesiones, etc. Esta escrito en ANSI C y es ofrecido como una solución de código abierto (Licencia BSD) por Redis Labs.
Redis incorpora además un interprete de Lua y escala horizontalmente bajo la filosofía de master-slave (maestro-esclavo). Permitiendo que un esclavo puede ser maestro para otros. Ofrece librerías clientes para los lenguajes más populares del mercado.

## ¿Qué es una caché Distribuida?
Una caché distribuida es una memoria caché compartida por varios servidores de aplicaciones, que normalmente se mantiene como un servicio externo a los servidores de aplicaciones que tienen acceso a ella.

## Ventajas de la caché
Mencionaremos algunas de las ventajas:
* Mejora del rendimiento de la aplicación.
    * No hay que estar consultando fuentes de acceso a datos en cada momento.
* Permite aumentar el throughput.
    * La cantidad de peticiones que se procesan por unidad de tiempo, si damos tiempos de respuesta más pequeños aumentamos la cantidad de operaciones que realizamos por unidad de tiempo.
* Reducción de procesamiento del lado de la BBDD.
* Desempeño predecible.
    * Los resultados están previamente guardados y el acceso es mucho más rápido que acceder a un recurso en la BBDD
* Control de Sesiones.

## Redis Sentinel vs Cluster

__Redis Sentinel__: Solución de alta disponibilidad de 3 nodos (1 primario y 2 secundarios). La conexión a la base se realiza consultando al agente de sentinel en que nodo se encuentra el primario.

__Redis Cluster__: Solución de alta disponibilidad de 6 nodos (3 primario y 3 secundarios). Los datos se dividen entre los 3 nodos primarios repartiendo la carga.