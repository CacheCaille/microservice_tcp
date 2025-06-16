# microservice_tcp

## Quels types de problèmes de concurrence peuvent apparaître dans ce système multi-clients ?
Il pourrait y avoir des problèmes lors de l'écriture dans les fichiers etat_serveur.json et serveur.log (accès conccurent sur un fichier) ou lors du changement du dictionnaire 
etat_serveur

## Que peut-il arriver si deux clients rejoignent ou quittent un canal en même temps ?
Des problèmes au niveau des utilisateurs présent dans un canal décrit dans le dictionnaire

## Votre système est-il vulnérable aux incohérences d’état ou aux conditions de course ? Comment s’en prémunir ?
Oui, il peut y avoir des problèmes. Il faudra sécuriser les sections critiques du code

