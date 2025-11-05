---
title: 'Remplacez votre IDE par un CDE !'
date: 2025-09-25T09:23:06+02:00
draft: false
cover:
  image: cde_cover.png
keywords: ['ide', 'cde', 'Eclipse che', 'devpod', 'codespace']
---

# Les Cloud Development Environments (CDE) - Embarquement immédiat !

Dans le monde du développement logiciel, l'onboarding des nouveaux développeurs peut être un véritable casse-tête. Installation des outils, configuration de l'environnement, résolution des incompatibilités... Autant de problèmes que les Cloud Development Environments (CDE) promettent de résoudre. Mais avant de plonger dans le monde des CDE, il est essentiel de comprendre les technologies qui les sous-tendent : les DevContainers et les DevFiles.

## DevContainers et DevFiles

### Qu'est-ce qu'un DevContainer ?

Un **DevContainer** est une spécification qui formalise la description d'un environnement de développement conteneurisé. Initiée par Microsoft en 2022, cette spécification est aujourd'hui adoptée par de nombreux IDE comme VS Code, IntelliJ et bien d'autres. Mais bien avant le financement massif par Microsoft (via Github, VSCode, ...), c'est **Red Hat** qui en était le précurseur dans le domaine avec ses [DevFile](https://devfile.io/) via le projet [Eclipse Che](https://eclipse.dev/che/) (adopté par la [CNCF](https://www.cncf.io/projects/devfile/) en 2022).

La philosophie est simple : développer dans un conteneur avec tous les outils nécessaires pré-installés et configurés. Cela permet de :
- Standardiser les environnements de développement entre tous les développeurs
- Garantir la reproductibilité de l'environnement
- Simplifier l'onboarding des nouvelles recrues

Pour en savoir plus, consultez les ressources officielles :
- Site officiel : https://containers.dev/
- Spécification : https://github.com/devcontainers/spec

### DevContainers vs Docker Compose : Quelle différence ?

On pourrait croire qu'un DevContainer n'est qu'un simple Docker Compose déguisé. En réalité, il s'agit d'une **couche d'abstraction** au-dessus de Docker (ou d'autres runtimes comme Podman) qui simplifie considérablement la construction et l'exécution du conteneur.

Les DevContainers apportent des fonctionnalités spécifiques au développement :
- Injection automatique de la configuration Git et SSH
- Installation de fonctionnalités supplémentaires via les "features"
- Montage automatique des volumes sources
- Gestion du cycle de vie (build, create, start, stop)

### Mise en oeuvre

Pour mettre en place un DevContainer, il suffit de créer un répertoire `.devcontainer` contenant un fichier `devcontainer.json` qui respecte la spécification. Voici la configuration minimale :

```json
{
	"name": "Spacesuits dev container",
	"image": "mcr.microsoft.com/devcontainers/java:1-21-bullseye"
}
```

Cette simple configuration suffit à démarrer ! Microsoft propose déjà plus d'une centaine d'images de base prêtes à l'emploi (cf. [templates pre-build](https://containers.dev/templates)).

### Les features : personnalisez votre environnement

Les **features** sont des scripts shell exécutés lors de la construction de l'image. Ils permettent d'installer des outils supplémentaires de manière déclarative :

```json
{
	"name": "Spacesuits dev container",
	"image": "mcr.microsoft.com/devcontainers/java:1-21-bullseye",
	"features": {
		"ghcr.io/devcontainers-extra/features/maven-sdkman:2": {
			"version": "3.9.11"
		}
	}
}
```

Plus d'[un millier de features](https://containers.dev/features) sont disponibles, et vous pouvez même créer les vôtres (privé ou partagé) !

### Configuration avancée

Les DevContainers permettent une configuration très fine de l'environnement. Notamment par des __hooks__ pour l'exécution de commandes aux différentes étapes de la construction de l'environnement.

```json
{
	"name": "Spacesuits dev container",
	"image": "mcr.microsoft.com/devcontainers/java:1-21-bullseye",
	"features": {
		"ghcr.io/devcontainers-extra/features/maven-sdkman:2": {
			"version": "3.9.11"
		}
	},
	"postCreateCommand": "cd backend/space-suit-back && mvn compile",
	"forwardPorts": [ 8080 ]
}
```

Cette configuration permet de :
- Exécuter des commandes après la création du conteneur
- Forwarder automatiquement les ports
- Définir des points de montage personnalisés
- Configurer des variables d'environnement

### Personnalisation de l'IDE

Les DevContainers permettent également de personnaliser l'IDE lui-même (structure spécifique à chaque outil implémentant la spécification : https://containers.dev/supporting) :

```json
{
	"customizations": {
		"vscode": {
			"settings": {
				"editor.tabSize": 2
			},
			"extensions": [
				"redhat.vscode-quarkus"
			]
		}
	}
}
```

Ainsi, tous les développeurs partagent non seulement le même environnement technique, mais aussi la même configuration d'IDE !

## Cloud Development Environments

### Histoire des CDE

Les environnements de développement dans le cloud ne datent pas d'hier :

#### Les Précurseurs (2010-2014)
- **Cloud9** (2010) : Premier IDE cloud majeur, révolutionnaire pour l'époque mais sans conteneurisation
- **Codeanywhere** (2013) : IDE collaboratif dans le navigateur

#### Le Game Changer
En 2014, [Codenvy](https://www.neosoft.fr/nos-publications/blog-tech/eclipse-che-un-ide-sur-le-cloud/), en se basant sur la spécification introduite par le projet open source
Eclipse Che, créé le premier IDE 100% orienté cloud. L'idée : pouvoir réaliser le dévelopement et la création d'un livrable uniquement sur des plateformes cloud.
C'est à cette période que le concept des **DevFiles** a été incubé. En 2019, Eclipse Che supporte officiellement les DevFiles et Codenvy est racheté par RedHat.

#### La Popularisation (2019-2022)
- **RedHat OpenShift Dev Spaces** (2019 - DevFiles)
- **Microsoft GitHub Codespaces** (2020 - DevContainers) : Adoption massive grâce à l'intégration avec GitHub et VScode
- **Google Cloud Workstations** (2022 - DevContainers)

### Comment fonctionnent les CDE ?

Il existe deux modes de fonctionnement principaux pour les CDE :

#### 1. À la Main

Avec des outils comme **DevPod** ou **Daytona**, le développeur lance lui-même son CDE. Ces solutions :
- Permettent de choisir le provider (environnement local, cloud, Kubernetes, etc.)
- Configurent automatiquement la connexion SSH
- Démarrent un serveur SSH chez le provider pour router les services
- Le backend de l'IDE tourne sur le provider, le frontend en local

[DevPod](https://devpod.sh/) est particulièrement intéressant car il fonctionne sans paywall et supporte de nombreux providers.

![devpod](devpod1.svg)
*Diagramme de composant de DevPod*

#### 2. À Distance

Des solutions comme [Coder](https://coder.com/), **Gitpod** (récemment renommé [Ona](https://ona.com/), [Theia Cloud](https://theia-cloud.io/) ou [Lapdev](https://lap.dev/) proposent une approche différente où tout est géré à travers une interface web.

**Coder** mérite une attention particulière car il peut être totalement self-hosted. Son architecture repose sur :
- Un service **coderd** qui gère la création des workspaces et la connexion des machines
- Des workspaces avec durées de vie limitées (configurables par l'admin)
- Une sauvegarde des modifications même si le workspace est arrêté

![coder diragram, ](coder-diagram.png)
*Diagramme d'infrastructure simplifié de Coder*

## CDE : Avantages et inconvénients

### Les inconvénients

Soyons honnêtes sur les contraintes !

- **📡 Accès au réseau** : Nécessite une connexion internet stable (les machines savent se reconnecter en cas de coupure)
- **🎛️ Complexité initiale** : Le setup initial peut être complexe selon les solutions
- **💰 Coût ou infrastructure dédiée** : Solutions SaaS coûteuses ou nécessité d'une infrastructure self-hosted
- **👶 Technologie récente** : Encore des progrès à faire sur l'intégration

### Les avantages

En revanche, es bénéfices sont nombreux :

- **⚡ Onboarding instantané** : Les nouveaux développeurs peuvent travailler dès le jour 0
- **🔄 Environnement reproductible** : Mêmes versions et configurations partout
- **🔒 Isolement** : Le code source est isolé sur un serveur (même s'il transite forcément sur le réseau)
- **🛠️ Configuration centralisée** : Gérée par l'équipe DevOps
- **💻 Machines légères** : Plus besoin de machines de guerre pour les développeurs

### Pour quel public ?

Les CDE sont particulièrement adaptés pour :

- **Grandes équipes** : Standardisation à grande échelle
- **Turnover important** : Onboarding simplifié
- **Configuration complexe** : Environnements difficiles à reproduire localement
- **Sécurité cruciale** : Code sensible qui ne doit pas quitter l'infrastructure
- **Formation technique** : Environnement éphémère préinstallé
- **Projets open-source** : Permet aux contributeurs de démarrer rapidement

## Conclusion

Les Cloud Development Environments représentent une évolution majeure dans la façon dont nous développons des logiciels. En s'appuyant sur les DevContainers et les DevFiles, ils offrent une solution élégante aux problèmes d'onboarding et de standardisation des environnements.

Que vous choisissiez une solution "à la main" comme DevPod pour garder le contrôle, ou une solution managée comme Coder ou GitHub Codespaces pour déléguer la gestion de l'infrastructure, les CDE promettent de transformer radicalement l'expérience de développement.

La technologie est encore jeune, mais son adoption croissante par les géants du cloud et les entreprises innovantes montre que nous ne faisons qu'entrevoir le potentiel des environnements de développement dans le cloud.

## Pour aller plus loin

- 🎬 [Développe sur un toaster grâce à Coder](https://www.youtube.com/watch?v=S9-C4ToXonw&list=PLuZ_sYdawLiWenx-X315dfZNOaliVnSTY/)
- 📄 [Documentation Coder Infrastructure](https://coder.com/docs/admin/infrastructure)
- 📄 [Introduction à Eclipse Che](https://eclipse.dev/che/docs/stable/overview/introduction-to-eclipse-che/)
- 📄 [Awesome DevContainers](https://github.com/manekinekko/awesome-devcontainers) - Liste de templates
- 🌐 [Spécification DevContainers](https://containers.dev/)
