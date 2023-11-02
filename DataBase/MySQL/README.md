[![en](https://img.shields.io/badge/lang-en-red.svg):ballot_box_with_check:](#) [![es](https://img.shields.io/badge/lang-es-yellow.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DataBase/MySQL/README.es.md)

# CheatSheets MySQL

## Table of Contents
- [Create user](#create-user)
- [Give permissions](#give-permissions)

## Create user
```mysql
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';
```

## Give permissions
```mysql
GRANT ALL PRIVILEGES ON * . * TO 'new_user'@'localhost';
```