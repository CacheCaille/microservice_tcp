# microservice_tcp

## Quels types de problèmes de concurrence peuvent apparaître dans ce système multi-clients ?
Il pourrait y avoir des problèmes lors de l'écriture dans les fichiers etat_serveur.json et serveur.log (accès conccurent sur un fichier) ou lors du changement du dictionnaire 
etat_serveur

## Que peut-il arriver si deux clients rejoignent ou quittent un canal en même temps ?
Des problèmes au niveau des utilisateurs présent dans un canal décrit dans le dictionnaire

## Votre système est-il vulnérable aux incohérences d’état ou aux conditions de course ? Comment s’en prémunir ?
Oui, il peut y avoir des problèmes. Il faudra sécuriser les sections critiques du code

## Quelles sont les grandes responsabilités fonctionnelles de votre application serveur (gestion client, traitement commande, envoi message, logs, persistance…) ?
L'authentification, la gestion de canaux, la messagerie

## Peut-on tracer une frontière claire entre logique métier et logique d’entrée/sortie réseau ?

## En cas d’erreur dans une commande, quelle couche doit réagir ?
C’est la couche de gestion de commande (dans handle) qui détecte les erreurs et écrit une réponse à l’utilisateur.

## Si vous deviez ajouter une nouvelle commande (ex : /topic, /invite, /ban), quelle partie du système est concernée ?
Il suffirait d’ajouter une nouvelle condition dans handle et de créer une méthode associée, éventuellement avec modifications mineures de l’état partagé.

## Que faudrait-il pour que ce serveur fonctionne à grande échelle (plusieurs centaines de clients) ?

## Quelles limitations structurelles du code actuel empêchent une montée en charge ?

## Ce serveur TCP pourrait-il être adapté en serveur HTTP ? Quelles parties seraient conservées, quelles parties changeraient ?
Il faudrait remplacer StreamRequestHandler par un framework HTTP, et la logique de traitement métier et l’état partagé peuvent être conservés en grande partie.

## Dans une perspective micro-services, quels modules seraient candidats naturels pour devenir des services indépendants ?




