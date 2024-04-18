---
title: 'CheatSheet'
date: 2024-04-17T22:27:52+02:00
genres: ['cheatsheet']
tags: ['dev', 'quickfix', 'markdown']
draft: true
---

## Introduction
J'ai quand même l'impression que c'est nettement plus facile d'écrire ce blog en markdown !

## Exemple
Le _code_ est plus joli ! [^1]

```java
String toto = "toto";
System.Out.println(toto);
```

> Et je peux faire ça !


```mermaid
sequenceDiagram
    participant Alice
    participant Bob
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->>John: Fight against hypochondria
    end
    Note right of John: Rational thoughts <br/>prevail!
    John-->>Alice: Great!
    John->>Bob: How about you?
    Bob-->>John: Jolly good!
```

## git
```mermaid
---
title: Example Git diagram
---
gitGraph
   commit
   commit
   branch develop
   checkout develop
   commit
   commit
   checkout main
   merge develop
   commit
   commit
```

## Image
On peut afficher une image aussi :
![An image](https://fujifilm-x.com/wp-content/uploads/2021/01/gfx100s_sample_04_thum-1.jpg)

[^1]: Et voilà la note !