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
Oui, elle est relativement claire : la classe IRCHandler gère les E/S réseau, tandis que les fonctions métier manipulent l’état du serveur.

## En cas d’erreur dans une commande, quelle couche doit réagir ?
C’est la couche de gestion de commande (dans handle) qui détecte les erreurs et écrit une réponse à l’utilisateur.

## Si vous deviez ajouter une nouvelle commande (ex : /topic, /invite, /ban), quelle partie du système est concernée ?
Il suffirait d’ajouter une nouvelle condition dans handle et de créer une méthode associée, éventuellement avec modifications mineures de l’état partagé.

## Que faudrait-il pour que ce serveur fonctionne à grande échelle (plusieurs centaines de clients) ?
le ThreadingTCPServer pourrait être un problème, la sérialisation du lock limite la concurrence, pas de file de messages tampon ni de gestion de surcharge réseau.

## Quelles limitations structurelles du code actuel empêchent une montée en charge ?
Le fait qu'il y ait une seule instance de lock pour tout l’état, la persistance rudimentaire (fichier JSON) non adaptée à une haute fréquence d’accès
et il n'y a aucune séparation forte entre protocoles, utilisateurs et canaux.

## Ce serveur TCP pourrait-il être adapté en serveur HTTP ? Quelles parties seraient conservées, quelles parties changeraient ?
Il faudrait remplacer StreamRequestHandler par un framework HTTP, et la logique de traitement métier et l’état partagé peuvent être conservés en grande partie.

## Dans une perspective micro-services, quels modules seraient candidats naturels pour devenir des services indépendants ?
les modules d'authentification utilisateurs, de gestion de canaux, des logs et des alertes par exemple.

## Est-il envisageable de découpler la gestion des utilisateurs de celle des canaux ? Comment ?
Possible avec des modules séparés ou micro-services

## Le serveur sait-il détecter une déconnexion brutale d’un client ? Peut-il s’en remettre ?
Oui, via la sortie du readline() (retourne une ligne vide).

## Si un message ne peut pas être livré à un client (socket cassée), le système le détecte-t-il ?
Les erreurs d’écriture sont silencieusement ignorées.

## Peut-on garantir une livraison ou au moins une trace fiable de ce qui a été tenté/envoyé ?
Il n'y a pas de garantie, il y a juste un log qui permet de garder une trace de ce qui a été tenté/envoyé.

## Quelles sont les règles implicites du protocole que vous utilisez ? Une ligne = une commande, avec un préfixe (/msg, /join, etc.) et éventuellement des arguments : est-ce un protocole explicite, documenté, formalisé ?
Une commande par ligne, préfixée par /, suivie éventuellement d’arguments (ex. /msg hello).

## Le protocole est-il robuste ? Que se passe-t-il si un utilisateur envoie /msg sans texte ? Ou un /join avec un nom de canal invalide ?
la commande /msg sans texte renvoie un message vide.
la commande /join accepte tout nom, même vide ou invalide.

## Peut-on imaginer une spécification formelle de ce protocole ? Un mini-ABNF, une doc à destination des développeurs de client ?
Oui on pourrait avoir une spécification pour ce protocole et réaliser une documentation serait utile.

## Quelle serait la différence structurelle entre ce protocole et un protocole REST ou HTTP ?
Le protocole actuel est a besoin d'une connexion persistante alors que HTTP est stateless, chaque requête est indépendante.






