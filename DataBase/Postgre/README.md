[![en](https://img.shields.io/badge/lang-en-red.svg):ballot_box_with_check:](#) [![es](https://img.shields.io/badge/lang-es-yellow.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DataBase/Postgre/README.es.md)

# CheatSheets Postgre

## Table of Contents
- [IDE](#ide)
- [Reset seed](#reset-seed)

## IDE
* https://www.pgadmin.org/
* https://www.postgresql.org/ftp/pgadmin/pgadmin4/v6.13/windows/

## Reset seed
```postgresql
TRUNCATE TABLE user.table  RESTART IDENTITY;
TRUNCATE TABLE user.table  RESTART IDENTITY CASCADE;
```