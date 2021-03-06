---
title: python
description: 
published: true
date: 2021-06-09T16:01:59.087Z
tags: 
editor: markdown
dateCreated: 2021-06-09T16:01:53.622Z
---

# Python Installation

PIP als modul nutzen um eine bestimmte Version zu benutzen

`python3 -m pip install acme==0.40.0`

## Installation unter CentOS 7

## Software Collections Aktivieren

Installation von scl

`sudo yum install centos-release-scl`

Installation von Python 3.6

`yum install rh-python36`

## Python Virtual Environment

Project Verzeichnis erstellen und hinein wechseln

```s
mkdir ~/my_new_project
cd ~/my_new_project
```

Python 3.6 mit dem _scl_ tool Aktiviren
`scl enable rh-python36 bash`

Innerhalb des Verzeichnisses ein Virtuelles Umgebung erstellen

`python -m venv my_project_venv`

Virtuelles Umgebung aktivieren

`source my_project_venv/bin/activate`

## [PIP](../pip.md)

## IDE Editoren

* [Jupyter](../jupyter)

## Error Fixing

* [rollbar](https://rollbar.com/)

## Template Engine

* [Jinja2]()
* [Pext Python-based extendable tool](https://github.com/Pext/Pext)
* [pexpect controlling other applications](http://pexpect.readthedocs.io/en/stable/examples.html)

## Web Framework

* [Flask](../flask)

## Infrastrukture Managemant

* [pyinfra automates service deployment](https://github.com/Fizzadar/pyinfra)

## authentication, authorization, und user management

* [okta](https://developer.okta.com/?widget=a)

## Full stack

* [Angular + Python + Flask — Full stack demo](https://medium.com/@balramchavan/angular-python-flask-full-stack-demo-27192b8de1a3)

## Uebersicht Zusammenstelungen

* [fullstackpython](https://www.fullstackpython.com/table-of-contents.html)