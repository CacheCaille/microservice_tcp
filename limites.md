# 2.1 Couplage et contrôle des responsabilités

## Quelles fonctions manipulent à la fois état, sockets et logs ?
La fonction envoyer_message fait tout cela. Elle accède à l’état, écrit dans les sockets et appelle log().

## Exemple de fonction qui viole la séparation des responsabilités ?
Oui, envoyer_alerte mélange logique métier, communication réseau et journalisation.

## Combien de fonctions métier à réécrire si on change de protocole ?
Presque toutes les fonctions de la classe IRCHandler, car elles sont liées aux sockets.

## Le protocole est-il une interface explicite ?
Non, il est implicite. Le format texte est directement lié au comportement.

## Quelles opérations sont atomiques ?
Aucune commande n’est garantie atomique. /msg peut être partiellement livré.
# 2.2 Protocole et interopérabilité

## Quels types d’erreurs peuvent être exprimés ?
Très peu. On distingue à peine les erreurs de syntaxe ou d’accès interdit.

## Peut-on formaliser le protocole ?
Oui, avec une grammaire simple comme : /commande [arguments].

## Comment différencier messages, alertes et infos système ?
Impossible automatiquement. Tout est du texte brut, sans balise ou type.

## Le protocole peut-il être versionné ?
Non, pas en l’état. Il n’y a pas de champ de version ni de compatibilité prévue.

## Coût d’ajouter une commande par rapport à une API REST ?
Plus risqué ici : tout est manuel, sans validation, sans test formel. Avec REST, c’est plus structuré.
# 2.3 Testabilité et fiabilité

## Quels tests unitaires sont impossibles sans simuler une socket ?
Toutes les commandes qui lisent ou écrivent dans wfile ou rfile dépendent des sockets.

## Comportements implicites non testés ?
Oui : un canal vide disparaît, une déconnexion n’est pas toujours visible, les messages peuvent arriver en désordre.

## Peut-on simuler un client malveillant avec /msg sans /join ?
Oui. Le serveur affiche une erreur mais ne plante pas. Il reste cohérent.

## Le système tolère-t-il les pannes partielles ?
Non. Si le fichier log échoue ou si un canal est corrompu, il n’y a pas de traitement d’erreur robuste.
# 2.3.1 Scalabilité et distribution

## L’état est-il réplicable ?
Non. L’état est en mémoire locale. Deux instances entreraient en conflit.

## Ressources globales vs distribuables ?
Les utilisateurs connectés sont globaux. Les messages par canal pourraient être distribués.

## Que faudrait-il pour une persistance robuste ?
Remplacer le fichier JSON par une base de données. Remplacer wfile par une file de messages.

## Le système supporte-t-il 1000 clients actifs ?
Non. Il utiliserait trop de threads, le verrou unique bloquerait, les sockets satureraient.

## Quelle architecture permettrait de tenir la charge ?
Une architecture à base de micro-services, avec des files (comme RabbitMQ) et une base distribuée.
# 2.3.2 Évolutivité du code et découpage en services

## Commandes déléguables à un service ?
/log, /alert, /msg pourraient être gérées par des services spécialisés.

## Y a-t-il des interfaces métier identifiables ?
Oui. La gestion des utilisateurs, des canaux et des messages sont trois interfaces possibles.

## Que manque-t-il pour le rendre DevOps-compatible ?
Des tests unitaires, des logs structurés, du monitoring, un fichier de conf, et une image Docker.

## Conditions pour devenir base micro-services ?
Il faut isoler les responsabilités, communiquer par messages, centraliser la persistance, ajouter du monitoring.
