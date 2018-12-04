---
type:               "post"
title:              "Retour sur le Forum PHP 2018"
date:               "2018-11-29"
publishdate:        "2018-11-29"
draft:              false
slug:               "forum-php-2018"
description:        "En attendant l'AFUP Day, voici notre retour sur le Forum PHP 2018."

thumbnail:          "/images/posts/thumbnails/forumphp2018-team.jpeg"
header_img:         "/images/posts/headers/forumphp2018-team.jpeg"
tags:               ["Développement", "Web", "afup", "Conférence", "ForumPHP"]
categories:         ["conference"]
author_username:    "elao"
co_authors:         ["rhanna", "aldeboissieu"]
---

Cette année, le Forum PHP s'est achevé sur l'annonce d'un nouvel évènement organisé par l'AFUP : les [Afup Days](https://event.afup.org/), qui auront lieu simultanément à Lille, Lyon et Rennes.
En attendant le 17 mai et la publication prochaine du programme, revenons sur le Forum PHP où une partie de l'équipe d'Elao s'est rendue.

## Nous avons aimé revenir aux fondamentaux
- <a href="https://www.youtube.com/watch?v=v3IPU3F_0JIY" target="_blank">Beyond design patterns and principles - writing good oo code]</a> par Matthias Noback


Ces rappels (ou découvertes pour certains) de l'utilisation de l'objet dans son code permet de revenir aux fondamentaux et d'ouvrir de nouvelles perspectives.

- Cessons les estimations par Frédéric Leguedois

Cette conférence nous a permis de questionner nos approches de l'agilité : les estimations sont-elles fiables ? Est-il vraiment possible de compter dessus ?
SCRUM est-elle vraiment une méthodologie agile? Autant de questions nécessaires pour réfléchir sur nos organisations au sein d'équipe de développement.
Au fait, [on en avait déjà parlé !](https://blog.elao.com/fr/elao/mixit-2018/#cessons-les-estimations)

- <a href="https://www.youtube.com/watch?v=aXq05_mdCqE" target="_blank">Comment j’ai commencé à aimer ce qu’ils appellent « design pattern »</a> par Samuel Roze

Ou comment les patrons de conception Adapter, EventDispatcher et Decorator sont concrètement utilisés pour avoir un code propre et découplé.

- <a href="https://www.youtube.com/watch?v=7TvIIt4c8uY" target="_blank">Générateurs et Programmation Asynchrone</a> par Benoît Viguier

Oui, on peut faire de l'asynchrone en PHP, et cela peut s'avérer très pratique.

## Nous avons aimé les retours d'expérience

- <a href="https://www.youtube.com/watch?v=Cq1sR005B2E" target="_blank">Docker en prod ? Oui, avec Kubernetes !</a> par Pascal Martin

Retour d'expérience très intéressant sur le passage à docker en prod d'une application à fort trafic (M6 Web).

- [Boostez vos applications avec HTTP2](https://www.youtube.com/watch?v=av9Z7NqMxFs) par Kevin Dunglas

Vous n'êtes pas encore passé au protocol HTTP2 sur vos application ? Kevin nous a convaincu en quelques simples arguments de faire la migration :

Activez-le une ligne dans NGINX: `listen 443 ssl http2;`, c'est sans impact sur vos applications PHP et vous bénéficiez d'emblée un gain de performance de l'ordre de x2 sur le temps de vos requêtes HTTP.

En plus de ça, vous serez prêt à utiliser les nouvelles fonctionnalités HTTP2 comme le `server_push` et le nouveau composant Symfony qui lui est dédié : [Mercure](https://github.com/symfony/mercure) !

En d'autres termes : pourquoi s'en priver ?

## Les conférences sur lesquelles on est pas très objectifs puisque nos collègues les ont données 😘
- [Voyage au centre du cerveau humain ou comment manipuler des données binaires en JavaScript](https://www.youtube.com/watch?v=LuCXFhaRXcw) par Thomas Jarrand

Thomas, développeur chez Elao, a partagé un retour d'expérience : charger un IRM dans le navigateur, pour les besoins d'une Université.
Tout en apportant des éléments concrets et une démo béton, Thomas a conquis son auditoire par son humour hors du commun 🤓

- [Mentorat & parcours de reconversion : comment faciliter l'apprentissage](https://www.youtube.com/watch?v=gW_TJ7kAu78) par Éric Daspet & Anne-Laure de Boissieu

Retour d'expérience sur plus d'un an et demi de mentorat. Concrètement, qu'est-ce que cela représente de mentorer, et qu'est-ce que cela peut vous apporter ?

- Lightning-talk : [Comment commander une application par le texte](https://www.youtube.com/watch?v=9tnkySxEoKA&feature=youtu.be&t=366) par Richard Hanna

Comment peut-on exploiter le routing de Symfony pour accéder rapidement aux ressources de son back-office ?
Richard nous a montré comment y parvenir et, en plus, accéder aux paramètres.

- Lightning-talk : [La randonnée à vélo 🚲](https://youtu.be/9tnkySxEoKA?t=701) par Thomas Jarrand

Et si les vacances démarraient en bas de chez vous, devant votre portail, avec un vélo et deux sacoches ?
C'est ce dont nous a parlé Thomas, qui nous montré que la rando vélo est accessible à tous et permet de découvrir des paysages insoupçonnés de l'Hexagone !

## Pour conclure

L'organisation était au top et les conférences de très bon niveau. Que demander de plus ? Ah oui, passer un moment agréable en retrouvant des connaissances ou en rencontrant de nouveaux pairs. Mission accomplie !
Nos prochains RDV (et on trépigne d'avance) : <a href="https://event.afup.org/" target="_blank">Afup Day</a> le 17 mai (Lyon, Rennes et Lille), et <a href="https://mixitconf.org/" target="_blank">MiXiT</a> à Lyon les 23 & 24 mai 2019 !
