[![en](https://img.shields.io/badge/lang-en-red.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DataBase/MySQL/README.md) [![es](https://img.shields.io/badge/lang-es-yellow.svg):ballot_box_with_check:](#)

# CheatSheets MySQL

## Tabla de Contenido
- [Crear usuario](#crear-usuario)
- [Dar permisos](#dar-permisos)

## Crear usuario
```mysql
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';
```

## Dar permisos
```mysql
GRANT ALL PRIVILEGES ON * . * TO 'new_user'@'localhost';
```