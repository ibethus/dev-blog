---
title: 'Mockoon : les bouchons contre-attaquent !'
date: 2025-04-14T09:23:06+02:00
draft: false
cover:
  image: mockoon_logo.png
---

Dans le développement moderne d'applications, l'interaction avec des webservices est devenue incontournable. Cependant, que se passe-t-il lorsque ces webservices ne sont pas disponibles, sont en cours de développement, ou simplement instables ? C'est là que Mockoon entre en scène pour sauver la situation.

## Qu'est-ce que Mockoon ?

Mockoon est un outil puissant qui permet de simuler des API REST avec une facilité déconcertante. Mais avant de plonger dans ses fonctionnalités, clarifions quelques concepts :

### Les bases du mock

Un **mock** (ou bouchon) est une imitation contrôlée d'un composant réel. Dans le contexte des APIs, il s'agit d'un serveur qui répond comme le ferait le véritable webservice, mais avec des données prédéfinies.

Mockoon se présente sous deux formes principales :
- **Une interface graphique (GUI)** : pour configurer visuellement vos mocks
- **Une interface en ligne de commande (CLI)** : pour l'intégration dans vos pipelines CI/CD ou environnements de déploiement

### L'interface graphique de Mockoon
![mockoon GUI](mockoon_gui.png)

L'interface de Mockoon est intuitive et organisée autour de concepts clés :

- **Environnements** : Espaces indépendants qui peuvent représenter différentes APIs ou contextes
- **Endpoints** : Les routes de votre API simulée, chacune configurée pour répondre à des requêtes spécifiques
- **Configuration des endpoints** : Méthode HTTP (GET, POST, etc.), chemin URL, en-têtes, et autres paramètres
- **Configuration des réponses** : Code de statut HTTP, en-têtes, contenu du corps (avec support de templates)

Cette interface permet de mettre en place un mock fonctionnel en quelques clics, sans écrire une seule ligne de code.

## Démonstration des fonctionnalités

### Import depuis Swagger/OpenAPI

L'un des atouts majeurs de Mockoon est sa capacité à importer des spécifications OpenAPI (anciennement Swagger). Cette fonctionnalité permet de :

1. Importer un fichier de spécification existant
2. Créer automatiquement toutes les routes définies
3. Générer des données aléatoires cohérentes grâce à [FakerJS](https://fakerjs.dev/)
4. Personnaliser ces réponses avec le système de templating intégré

{{ $url := "spacesuit-api.yaml" }}
{{ $api := "" }}
{{ with try (resources.GetRemote $url) }}
  {{ with .Err }}
    {{ errorf "%s" . }}
  {{ else with .Value }}
    {{ $api = . | openapi3.Unmarshal }}
  {{ else }}
    {{ errorf "Unable to get remote resource %q" $url }}
  {{ end }}
{{ end }}

### Data Buckets

Les data buckets sont des réservoirs de données qui permettent de stocker et de réutiliser des informations à travers différentes routes. Ils sont particulièrement utiles pour :

- Maintenir la cohérence des données entre les appels
- Implémenter des fonctionnalités comme la pagination
- Simuler des opérations CRUD complètes

### Mode Proxy

Que faire si vous ne disposez pas de la documentation OpenAPI d'un service existant ? Mockoon propose un mode proxy qui :

1. Redirige les requêtes vers le webservice réel
2. Enregistre automatiquement les réponses
3. Permet de transformer ces réponses en templates réutilisables

Ce mode est idéal pour capturer le comportement d'APIs existantes avant de les simuler localement.

### Règles et filtres

Mockoon ne se contente pas de réponses statiques. Ses règles permettent de :

- Filtrer les requêtes selon des paramètres spécifiques
- Analyser et répondre en fonction du contenu d'un token JWT
- Traiter des enveloppes SOAP pour les services web plus traditionnels
- Personnaliser dynamiquement vos réponses selon le contexte

## Déploiement du serveur de mock

Une fois votre configuration de mock prête, Mockoon propose plusieurs options de déploiement :

### Via npm

```bash
# Installation
npm install -g @mockoon/cli

# Démarrage avec un ou plusieurs fichiers de configuration
mockoon-cli start --data clones.json
```

Pour des configurations plus complexes :

```bash
mockoon-cli start \
 --data clones.json planetes.json \
 --port 3000 3001
```

### Via Docker

Mockoon permet de générer un Dockerfile adapté à votre configuration :

```bash
mockoon-cli dockerize \
 --data clones.json planetes.json \
 --port 3000 3001 \
 --output ./Dockerfile
```

Puis de le construire et l'exécuter :

```bash
docker build -t clones_mocks .
docker run -d -p 3000:3000 -p 3001:3001 clones_mocks
```

### Via GitHub Actions

Mockoon propose également une action GitHub officielle : `mockoon/cli-action@v2`

## Mockoon vs Wiremock

Si vous connaissez déjà [Wiremock](https://wiremock.org/), vous vous demandez peut-être pourquoi choisir Mockoon ?

### Avantages de Mockoon
- ✅ Plus rapide à mettre en place
- ✅ Plus simple à configurer grâce à son interface graphique intuitive

### Inconvénients de Mockoon
- ❌ Moins de fonctionnalités avancées que Wiremock (mais couvre largement les besoins courants)

## Contribuer au projet

Mockoon est un projet open-source en constante évolution. Vous pouvez contribuer ou suivre son développement sur [GitHub](https://github.com/mockoon).

Le projet intègre déjà de nombreuses fonctionnalités essentielles :
- Support des expressions régulières pour les routes
- Système de templating avancé
- Support automatique de CORS
- Import/export OpenAPI
- CLI complète
- En-têtes de réponse personnalisables
- Latence simulée
- Webhooks et callbacks
- Serveur de fichiers statiques
- Support des WebSockets
- Support SOAP
- TLS
- Variables d'environnement
- Data buckets
- Endpoint d'administration

## Conclusion

Mockoon représente une solution élégante et efficace pour le mocking d'API, accessible tant aux débutants qu'aux développeurs expérimentés. Sa flexibilité, sa simplicité et sa puissance en font un outil incontournable dans l'arsenal de tout développeur travaillant avec des APIs.

Que vous cherchiez à développer votre frontend indépendamment du backend, à tester des scénarios spécifiques, ou simplement à travailler sans connexion internet, Mockoon est là pour vous faciliter la tâche — comme un fidèle compagnon dans votre voyage de développement à travers la galaxie du code.