---
title: mysql-mgmnt
description: 
published: true
date: 2021-06-09T15:40:55.807Z
tags: 
editor: markdown
dateCreated: 2021-06-09T15:40:50.519Z
---

# Datenbanken

## Neue Datenbank anlegen

`CREATE DATABASE database_name;`

## Neue Datenbank anlegen wenn Sie nicht schonn vorhanden ist

`CREATE DATABASE IF NOT EXISTS database_name;`

## Liste der Datenbanken

`SHOW DATABASES;`

## Datenbank löschen

`DROP DATABASE database_name;`

## Datenbank löschen wenn Sie vorhanden ist

`DROP DATABASE IF EXISTS database_name;`

## Benutzer

### Neuen Benutzer anlegen

`CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'user_password';`

### Benutzer Password ändern. wenn DB älter sind

`SET PASSWORD FOR 'database_user'@'localhost' = PASSWORD('new_password');;`

### Neuen Benutzer anlegen auf ip eingeschränkt

`CREATE USER 'newuser'@'10.8.0.5' IDENTIFIED BY 'user_password';`

### Alle Benutzer auflisten

`SELECT user, host FROM mysql.user;`

### Benutzerberechtigugen ausgeben

`SHOW GRANTS FOR 'database_user'@'localhost';`
