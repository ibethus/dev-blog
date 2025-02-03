---
title: 'La puissance du SVG pour le web'
date: 2025-02-03T18:20:06+02:00
draft: false
---

## Introduction

Les images SVG (Scalable Vector Graphics) sont largement utilisées sur le web en raison de leur capacité à être redimensionnées sans perte de qualité. Il s'agit d'un fichier au format xml, aisément inclu et modifié dans une page web.  
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
 <svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="0" width="100" height="100" fill="lightblue"/>
    <a href="https://example.com">
        <rect x="25" y="25" width="50" height="50" fill="lightgreen"/>
        <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="5px">Click me!</text>
    </a>
</svg>

> En prime, le navigateur interprête bien le texte comme un lien, d'où le soulignage lors du "hover"

## Changer dynamiquement le style des images SVG
De la même manière, il est tout à fait possible d'utiliser des sélecteurs css classiques pour appliquer des styles à chaque élément du svg, par exemple, une couleur :

```html
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <a href="https://example.com">
    <rect x="10" y="10" width="180" height="180" fill="lightblue" />
  </a>
</svg>
 <style>
    svg rect:hover {
      fill: orange;
    }
</style>
```
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <a href="https://example.com">
    <rect x="10" y="10" width="180" height="180" fill="lightblue" />
  </a>
</svg>
 <style>
    svg rect:hover {
      fill: orange;
    }
</style>

On peut aller plus loin avec des images plus complexes génerées à partir d'un logiciel de dessin svg (par exemple photopea.com). Notons que le style du svg est inclu par le logiciel directement dans les balises.

```html
<svg version="1.2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">	
	<style>
		.default { fill: #bc2424 } 
	</style>
	<path id="trait1" class="default" d="m284.6 126.1l-0.9 4.9-197.7-35.7 0.9-4.9z"/>
	<path id="trait2" class="default" d="m281.5 128.9l4.3 2.5-88.6 153.4-4.3-2.5z"/>
	<path id="trait3" class="default" d="m82 100l3.5-3.5 105.8 192.5-3.6 3.5z"/>
	<path id="rond1" class="default" d="m87.5 127c-18 0-32.5-14.5-32.5-32.5 0-18 14.5-32.5 32.5-32.5 18 0 32.5 14.5 32.5 32.5 0 18-14.5 32.5-32.5 32.5z"/>
	<path id="rond2" class="default" d="m282 151c-14.4 0-26-11.6-26-26 0-14.4 11.6-26 26-26 14.4 0 26 11.6 26 26 0 14.4-11.6 26-26 26z"/>
	<path id="rond3" class="default" d="m187 342c-27.7 0-50-22.4-50-50 0-27.6 22.3-50 50-50 27.7 0 50 22.4 50 50 0 27.6-22.3 50-50 50z"/>
</svg>
```
<svg version="1.2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">	
	<style>
		.bulle, .trait { fill: orange } 
	</style>
	<path id="trait1" class="trait" d="m284.6 126.1l-0.9 4.9-197.7-35.7 0.9-4.9z"/>
	<path id="trait2" class="trait" d="m281.5 128.9l4.3 2.5-88.6 153.4-4.3-2.5z"/>
	<path id="trait3" class="trait" d="m82 100l3.5-3.5 105.8 192.5-3.6 3.5z"/>
	<path id="rond1" class="bulle" d="m87.5 127c-18 0-32.5-14.5-32.5-32.5 0-18 14.5-32.5 32.5-32.5 18 0 32.5 14.5 32.5 32.5 0 18-14.5 32.5-32.5 32.5z"/>
	<path id="rond2" class="bulle" d="m282 151c-14.4 0-26-11.6-26-26 0-14.4 11.6-26 26-26 14.4 0 26 11.6 26 26 0 14.4-11.6 26-26 26z"/>
	<path id="rond3" class="bulle" d="m187 342c-27.7 0-50-22.4-50-50 0-27.6 22.3-50 50-50 27.7 0 50 22.4 50 50 0 27.6-22.3 50-50 50z"/>
    </path>
    
</svg>
<style>
    .bulle:hover{
        fill: lightblue;
    }
    #rond1 ~ .trait{
        fill: lightblue;
    }
</style>

## Rendre les images SVG responsives
Pour rendre une image SVG responsive, nous devons utiliser les attributs viewBox et preserveAspectRatio. 

Voici un exemple :

```html
<svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <a href="https://example.com">
        <rect x="10" y="10" width="180" height="180" fill="lightblue" />
        <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="24px">Click me!</text>
    </a>
</svg>
```

Dans cet exemple, l'attribut viewBox définit un système de coordonnées pour le contenu SVG, et preserveAspectRatio assure que l'image conserve ses proportions lorsque la taille de la fenêtre change.

Conclusion
Les images SVG offrent une grande flexibilité pour les développeurs web. En utilisant des éléments `<a>`, des styles dynamiques et des techniques de mise en page responsive, vous pouvez créer des graphiques interactifs et adaptatifs pour vos projets web. Essayez ces techniques dans vos propres projets pour voir comment elles peuvent améliorer l'expérience utilisateur.