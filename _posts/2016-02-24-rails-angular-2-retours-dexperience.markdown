---
layout: post
title: Rails + Angular 2, retours d'expérience
date: '2016-02-24 03:47:00'
tags:
- javascript
- typescript
- angular-2
- webpack
- ror
---

Je me suis lancé depuis 1 mois dans la réécriture d'un dashboard codé avec le framework Angular, de la version 1.x vers la nouvelle version 2, couramment en beta.

Cela m'a permis de découvrir notamment [Angular 2](https://angular.io), [TypeScript](http://www.typescriptlang.org) et leurs intégration avec le framework Ruby On Rails. 

Je souhaite exprimer mes premières impressions et les difficultés que j'ai eu tout au long de ce développement.  

## Environment

L'application de départ est une application web développée avec Ruby On Rails. Un utilisateur se log et accède à un dashboard composé de nombreux widgets lui permettant de visualiser simplement les caractéristiques de son réseau. 

Ce **dashboard** est une page html servie par Ruby On Rails qui contient une **application Angular 1.x**. L'application front-end se concentre sur l'UX, la logique étant déléguée pour la plupart au serveur. 

Le développement de ce dashboard fut laborieux, tout simplement parce que le **fonctionnement d'Angular 1 est compliqué à comprendre** (surtout pour le designer de l'équipe) et résulte en un **gros spaghetti d'émissions et d'interceptions d'événements pour la communication entre les différents widgets**. Il faut un certain niveau en JavaScript pour comprendre l'application, ce qui veut dire que peu de personnes sont capables de développer de nouvelles tâches ou de faire du refactoring dans notre équipe. 

J'ai découvert Angular 2 au moment de la sortie de la version alpha et à la lecture du tutoriel, ai décidé de l'utiliser pour notre dashboard, dans un premier pour avoir de **meilleures performances et une meilleure UX**.  

## TypeScript

On peut développer en TypeScript pour Angular 2. J'ai beaucoup d'expérience avec JavaScript et CoffeeScript et ai été rapidement productif avec TypeScript. 

J'adore la possibilité d'**utiliser la syntaxe d'ES2016 et des futures versions de JS**, le déclaration de **types de données qui accélère le développement et limite les erreurs**, notamment lorsque l'on accède à des propriétés d'objets en cascade. Je trouve le **code** clean, proche de JS et **compréhensible par un plus grand nombre de développeurs**, Ouf ! Les collègues connaissant Java vont pouvoir me donner un coup de main.  

**J'utiliserai donc en priorité TypeScript** pour mes futures développement en JS.

## Angular 2

J'aime la nouvelle organisation de mon code avec l'utilisation des **web components**. Un web component devient alors une simple classe qui regroupe logique, vue et style. C'est vraiment très proche de ce que je codais naturellement avant l'utilisation des controllers, des scopes etc.. de la version 1.

On construit son application sur une hiérarchie de composants, j'ai eu du mal avec la partie communication entre composants et ai abandonné le mauvais réflexe de l'utilisation d'événements. Je vous renvoie à ces pages concernant la communication entre composants:

* [data-bound input property](https://angular.io/docs/ts/latest/api/core/Input-var.html)
* [référence à d'autres composants](https://angular.io/docs/ts/latest/api/core/forwardRef-function.html)

La syntax au niveau des vues est très similaire et fait la distinction entre les 2 sens de flow des données. Cela demande un peu d'effort pour la syntaxe comparé à Angular 1.x mais vient très rapidement.

## Intégration avec Rails

Ceci a été l'étape la plus dure, puisque Angular 2 n'est pas encore en version stable et il n'existe pas pour l'heure d'intégration simple avec l'*asset pipeline*. J'ai donc utiliser un environnement Node.js et [Webpack](https://webpack.github.io/) pour développer mon application front_end et générer mes assets représentant les librairies 3rd party et mon application.

Puis, au niveau de la view, j'inclue dynamiquement avec une balise *script* mes assets sans passer par l'*asset pipeline* (due à des problèmes de *minification* du code JS). 

Je me propose de décrire plus en détails ce passage et de donner des exemples de codes dans un prochain post.