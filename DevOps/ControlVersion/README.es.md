[![en](https://img.shields.io/badge/lang-en-red.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/README.md) [![es](https://img.shields.io/badge/lang-es-yellow.svg):ballot_box_with_check:](#)

# CheatSheets Control de Versiones

## Tabla de Contenido
- [Web Semantic Version](#web-semantic-version)
- [Estrategias de ramificación de Git](#estrategias-de-ramificación-de-git)
- [Tortoise GIT configuración Windows](#tortoise-git-configuración-windows)
- [TFS Eliminar un workspace](#tfs-eliminar-un-workspace)

## Web Semantic Version
* Click aquí [semver.org](https://semver.org/)


## Estrategias de ramificación de Git

### GitHub Flow

__Es ideal para organizaciones que necesitan simplicidad e implementación frecuente.__ 
Cada trabajo, ya sea una corrección de errores o una característica, se realiza a través de una rama que se crea desde master. Una vez completado el trabajo en la rama, se revisa y prueba antes de fusionarlo con la rama master y enviarlo a producción.

<img style="background-color:white;" src='https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/git-branching-strategies--github-flow.png?raw=true' />

### Git Flow
__Es ideal para proyectos que tienen un ciclo de lanzamiento programado.__
Consta de dos ramas "principales" que duran para siempre. Estas son master y develop. 
* La rama master es la rama principal donde el código fuente siempre refleja un estado listo para producción (por ejemplo, sus releases).
* La rama develop siempre tiene los últimos cambios que están listos para probar en un release (no es estable).

<img style="background-color:white;" src='https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/git-branching-strategies--git-flow.png?raw=true' />

### GitLab Flow
__Este es el flujo de trabajo ideal para organizaciones que necesitan realizar lanzamientos con frecuencia, en lugar de tener lanzamientos programados.__

A diferencia de los otros flujos donde en master estaba la rama que representa el código que está en el servidor de producción, este flujo tiene una rama dedicada __production__ que sirve para ese propósito. Además, se incluye una rama de __staging__ para representar su entorno de ensayo al que ingresa para pruebas mas exhaustivas e incluso pruebas de usuarios finales. Esto significa que debes tener al menos 3 branch principales:

* master: Branch del entorno de desarrollo local de todos.
* staging: aquí es donde se fusiona el master para las pruebas antes de pasar a producción.
* production: este es el código de producción etiquetado con el release.

<img style="background-color:white;" src='https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/git-branching-strategies--gitlab-flow.png?raw=true' />

#### References:
* https://blog.programster.org/git-workflows
* https://www.linkedin.com/posts/nikkisiapno_git-branching-strategies-explained-a-well-planned-activity-7119985969348448256--Kgo


## Tortoise GIT configuración Windows

1. Ejecutar la consola __CMD.exe__ y ubicarse en la carpeta C:\Users\\[myUser]
2. Ejecutar el comando:
    * ssh-keygen -t ed25519 -C "my_mail@gmail.com"
    (el mail debe ser el utilizado en github)
3. Ejecutar la App __puttyGen__ :arrow_right: conversions :arrow_right: import key :arrow_right: 
C:\Users\\[myUser]\\.ssh\id_ed25519 :arrow_right: Save privateKey :arrow_right: name id_ed25519_ppk
4. En https://github.com/settings/keys 
    click __New SSH Key__ y copiar el contenido del archivo:
    * C:\Users\\[myUser]\\.ssh\id_ed25519.pub
5. Ir al repositorio y copiar la linea para clonar el repo, pero habiendo seleccionado SSH. 
Ejemplo: git@github.com:SpaikSaucus/cheatsheets.git
6. Ir a nuestro workspace y hacer click derecho, git clone y tildar __LoadPuttyKey__ para cargar el archivo generado en el paso 3:
    * C:\Users\[myUser]\.ssh\id_ed25519_ppk.ppk
7. En el push, hacer click en __Autoload Putty Key__.


## TFS Eliminar un workspace 
__Requiere permisos administrador en el TFS__
* C:\Program Files\Microsoft Visual Studio 14.0>tf workspace /delete "C0125826E;UserPerez"


