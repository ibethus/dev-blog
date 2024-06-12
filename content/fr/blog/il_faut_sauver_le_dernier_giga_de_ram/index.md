+++
title = 'Il faut sauver le dernier giga de RAM'
date = 2024-06-12T10:22:53+02:00
draft = true
+++

Cet article présente un retour d'expérience sur une mission d'amélioration d'une application Java ancienne. L'objectif est d'en ressortir des éléments réexploitables sur d'autres projets, pour vous aider à dépoussierer et rebooster vos vieilles applications Java/Spring. 


## Introduction
Depuis quelque temps, je travaille sur une chaîne de post-production [éditique](https://fr.wikipedia.org/wiki/%C3%89ditique). Périodiquement, notre chaîne subit des pics de charge importants, qui entrainent une surconsommation de RAM sur les serveurs du client. 

Cette année, c'en est trop, le client refuse d'augmenter -de nouveau- la capacité de ses serveurs, et nous demande, à moi et à l'architecte applicatif du projet, de réduire coûte que coûte la consommation de RAM générale de la chaîne. 

Problème : il n'y a ni tests de performance, ni metrics ou observabilité, pas d'accès aux environnements de production et plusieurs dizaines de composants à analyser. Nous avons environ trois semaines pour trouver une solution. 

## Quelle démarche adopter ?
Notre chaîne batch fonctionne de la manière suivante : elle ingère une archive contenant des miliers de couriers, sous la forme de fichiers imprimables, puis procède à une longue série de traitements permetant de préparer l'impression et le tracking de ces courriers. 

Cette année, le pic d'activité concerne le traitement d'environ cinq milions de couriers. De telles volumétrie étant impossible à traiter sur nos environnements de developpement, nous prenons la décision de demander au client une archive de test de cent mille couriers. Nous espérons que le traitement de cet échantillon permette de reproduire les comportements problématiques de la production. Ainsi, l'extrapolation de nos résultats sur l'échantillon devrait donner une idée des résultats sur les vraies données. 

## Analyse
Une fois l'archive de test récupérée, il convient de l'injecter dans la chaîne pour cibler les goulots d'étranglement.

### Détecter les composants problématiques
Notre flux de test était de taille raisonnable, nous baissons le `Xmx` de nos composants Java afin de déterminer lesquels sont les plus prompts à tomber en erreur. 

#### Le Xmx ? 
Le [Xmx](https://www.baeldung.com/jvm-parameters#explicit-heap-memory---xms-and-xmx-options) est une option primordiale à connaitre et à maitriser lors du lancement d'une JVM. C'est elle qui définit la **taille maximale de la heap autorisée pour le programme**. Si le programme tente d'allouer plus de mémoire dans la heap, la JVM stoppe son exécution et lève une `java.lang.OutOfMemoryError: Java heap space`[^1].

> Utilisation : `java -Xmx2048m -jar {monJar}`

Grâce à cette méthode, nous sommes rapidemment capable de détecter quelques modules dont la consommation de RAM semble problémtique. 

### Récolter des données avec des profilers

### Analyser la consommation mémoire
### Analyse des requêtes SQL générées

## Actions
### Optimisations hibernate
### Nettoyer son code Java legacy
### Quelques méthodes pour optimiser ses requêtes SQL

## Conclusion

[^1]: [Documentation Oracle](https://docs.oracle.com/javase%2F8%2Fdocs%2Fapi%2F%2F/java/lang/OutOfMemoryError.html)