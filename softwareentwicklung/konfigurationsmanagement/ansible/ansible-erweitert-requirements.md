---
title: ansible-erweitert-requirements
description: 
published: true
date: 2021-06-09T14:55:03.955Z
tags: 
editor: markdown
dateCreated: 2021-06-09T14:54:58.323Z
---

# Ansible Galaxy requirements

In der Datei `requirements.yml` kann man seine abhängigkeiten definieren `ansible-galaxy` kümmert sich dann darum alle rolen die benötigt werden auch  zu laden.

Die datei `requirements.yml` liegt im quell Verzeichnis

Die abhängigkeiten können wie folded aufgelöst werden.

```sh
ansible-galaxy install -r requirements.yml
```

Hier ist eine Beispiel wie eine solche datei aufgebaut ist

```sh
---
- src: https://github.com/myprojekt/ansible-apache2.git
- src: https://github.com/myprojekt/ansible-mariadb-mysql.git
- src: https://github.com/myprojekt/ansible-mariadb-galera-cluster.git
- src: https://github.com/myprojekt/ansible-powerdns.git
```

In diesem Beispiel muss `- src:` eine erreichbare http|https Quelle sein.
Alternativ sind aber alternative quellen möglich.
