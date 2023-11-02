[![en](https://img.shields.io/badge/lang-en-red.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DataBase/Postgre/README.md) [![es](https://img.shields.io/badge/lang-es-yellow.svg):ballot_box_with_check:](#)

# CheatSheets Postgre

## Tabla de Contenido
- [IDE](#ide)
- [Resetear semilla](#resetear-semilla)

## IDE
* https://www.pgadmin.org/
* https://www.postgresql.org/ftp/pgadmin/pgadmin4/v6.13/windows/

## Resetear semilla
```postgresql
TRUNCATE TABLE user.table  RESTART IDENTITY;
TRUNCATE TABLE user.table  RESTART IDENTITY CASCADE;
```