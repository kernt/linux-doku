---
title: elk-stack-logstash-konfiguration
description: 
published: true
date: 2021-06-09T15:19:27.438Z
tags: 
editor: markdown
dateCreated: 2021-06-09T15:19:22.161Z
---

Eine allgemeine Logstash-Plugin-Konfiguration wie folgt aus:
```
input {
  
}

filter {
  
}

output {
  
}
```

Eine Logstash-Konfiguration besteht aus einer Reihe von `input`-, `filter`- und `output`-Plugins und deren entsprechenden Eigenschaften. Jedes Plugin spielt eine wichtige Rolle beim Analysieren, Verarbeiten und schließlich beim Einlegen der Daten in das erforderliche Format. Input-Plugins generieren das Ereignis, Filter modifizieren sie, und die Ausgabe liefert sie an andere Systeme.

![Logstash plugins](https://www.packtpub.com/graphics/9781787288546/graphics/Ch03_01.jpg)