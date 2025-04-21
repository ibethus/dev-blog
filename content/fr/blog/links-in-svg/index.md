---
title: 'La puissance du SVG pour le web'
date: 2025-02-03T18:20:06+02:00
draft: false
keywords: ['svg', 'web-development', 'html', 'css', 'responsive-design', 'interactivity', 'vector-graphics', 'frontend']
---

 <style>
    a:hover{
        text-decoration: underline;
    }
</style>

## Introduction

Les images SVG (Scalable Vector Graphics) sont largement utilisées sur le web en raison de leur capacité à être redimensionnées sans perte de qualité. Il s'agit d'un fichier au format xml, aisément inclut et modifié dans une page web.  
Dans cet article, nous allons explorer comment ajouter des liens HTML dans ces images, changer dynamiquement leur style en css et enfin comment les rendre _responsives_.

## Inclure des liens HTML dans des images SVG

Pour inclure des liens HTML dans une image SVG, rien de plus simple. Ajouter simplement la balise `<a>` sur n'importe quel élément du svg le transforme en lien cliquable :

```html
 <svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="0" width="100" height="100" fill="lightblue"/>
    <a href="https://example.com">
        <rect x="25" y="25" width="50" height="50" fill="lightgreen"/>
        <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="5px">Click me!</text>
    </a>
</svg>
```
 <svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="0" width="200" height="200" fill="lightblue"/>
    <a href="https://example.com">
        <rect x="50" y="50" width="100" height="100" fill="lightgreen"/>
        <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="10px">Click me!</text>
    </a>   
</svg>

## Changer dynamiquement le style des images SVG
De la même manière, il est tout à fait possible d'utiliser des sélecteurs css classiques pour appliquer des styles à chaque élément du svg, par exemple, une couleur. Notons que le style du svg est inclut par le logiciel de création de svg directement dans les balises (ici [photopea.com](photopea.com)).

```html
<svg version="1.2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">	
	<style>
		.bulle { fill: #bc2424 } 
	</style>
	<path id="trait1" class="trait" d="m284.6 126.1l-0.9 4.9-197.7-35.7 0.9-4.9z"/>
	<path id="trait2" class="trait" d="m281.5 128.9l4.3 2.5-88.6 153.4-4.3-2.5z"/>
	<path id="trait3" class="trait" d="m82 100l3.5-3.5 105.8 192.5-3.6 3.5z"/>
	<path id="rond1" class="bulle" d="m87.5 127c-18 0-32.5-14.5-32.5-32.5 0-18 14.5-32.5 32.5-32.5 18 0 32.5 14.5 32.5 32.5 0 18-14.5 32.5-32.5 32.5z"/>
	<path id="rond2" class="bulle" d="m282 151c-14.4 0-26-11.6-26-26 0-14.4 11.6-26 26-26 14.4 0 26 11.6 26 26 0 14.4-11.6 26-26 26z"/>
	<path id="rond3" class="bulle" d="m187 342c-27.7 0-50-22.4-50-50 0-27.6 22.3-50 50-50 27.7 0 50 22.4 50 50 0 27.6-22.3 50-50 50z"/>
</svg>
<style>
    .bulle:hover{
        fill: lightblue;
    }
</style>
```

<svg version="1.2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">	
	<style>
		.bulle { fill: #bc2424 } 
	</style>
	<path id="trait1" class="trait" d="m284.6 126.1l-0.9 4.9-197.7-35.7 0.9-4.9z"/>
	<path id="trait2" class="trait" d="m281.5 128.9l4.3 2.5-88.6 153.4-4.3-2.5z"/>
	<path id="trait3" class="trait" d="m82 100l3.5-3.5 105.8 192.5-3.6 3.5z"/>
	<path id="rond1" class="bulle" d="m87.5 127c-18 0-32.5-14.5-32.5-32.5 0-18 14.5-32.5 32.5-32.5 18 0 32.5 14.5 32.5 32.5 0 18-14.5 32.5-32.5 32.5z"/>
	<path id="rond2" class="bulle" d="m282 151c-14.4 0-26-11.6-26-26 0-14.4 11.6-26 26-26 14.4 0 26 11.6 26 26 0 14.4-11.6 26-26 26z"/>
	<path id="rond3" class="bulle" d="m187 342c-27.7 0-50-22.4-50-50 0-27.6 22.3-50 50-50 27.7 0 50 22.4 50 50 0 27.6-22.3 50-50 50z"/>
</svg>
<style>
    .bulle:hover{
        fill: lightblue;
    }
</style>

## Rendre les images SVG responsives

Le SVG est un format qui s'adapte très facilement aux contraintes de tailles des écrans. 

Pour rendre une image SVG responsive, on utilise deux attributs : `viewBox` et `preserveAspectRatio`. Ils vont nous permettre de créer des SVG qui s'adaptent élégamment à n'importe quelle taille d'écran.

Jetons un œil à cet exemple :

```html
<svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <a href="https://example.com">
        <rect x="10" y="10" width="80" height="80" fill="lightblue" />
        <text x="50" y="50" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="10px">Click me!</text>
    </a>
</svg>
```

<svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <a href="https://example.com">
        <rect x="10" y="10" width="80" height="80" fill="lightblue" />
        <text x="50" y="50" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="10px">Click me!</text>
    </a>
</svg>

> Essayez de redimensionner la fenêtre pour voir le svg s'adapter

Dans ce code, `viewBox="0 0 100 100"` définit notre "monde SVG" - une toile virtuelle de 100×100 unités où nos éléments vont vivre, peu importe la taille réelle à l'écran.

Quant à l'attribut `preserveAspectRatio="xMidYMid meet"`, il garantit que notre SVG reste toujours bien proportionné.

## Conclusion : Le SVG, votre allié de choix pour le web moderne

Le SVG, c'est le format d'image du web par excellence ! Récapitulons pourquoi vous devriez l'adopter dès aujourd'hui :

- **Interactivité** : Ajoutez facilement des liens, des zones cliquables et des interactions utilisateur
- **Style dynamique** : Modifiez les couleurs, les formes et les transitions directement via CSS
- **100% responsive** : Une image qui s'adapte parfaitement à tous les écrans, du smartphone à l'écran 4K
- **Extra léger** : Des fichiers bien plus légers que les PNG ou JPG équivalents

Contrairement aux formats bitmap traditionnels, le SVG vous offre un contrôle total sur chaque élément de votre image. Imaginez pouvoir changer la couleur d'un logo au survol, animer un graphique ou rendre certaines parties cliquables - tout ça sans toucher à Photoshop !

Prêt.e à vous lancer ? Essayez dès maintenant d'intégrer un SVG interactif dans votre prochain projet et vous verrez la différence. Vos utilisateurs (et votre portfolio) vous diront merci ! 😉