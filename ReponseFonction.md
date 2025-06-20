### 1. Qui traite les commandes ?
- Quelle fonction interprète /msg, /join, etc. ?
  - La fonction handle de la classe IRCHandler inteprète les commandes reçues.
- Qui accède à la mémoire partagée etat_serveur ?
  - Toutes les méthodes de IRCHandler : set_pseudo(), rejoindre_canal(), envoyer_message(), envoyer_alerte()
  - Les fonctions globales log() et broadcast_system_message()
### 2. Où sont stockées les infos ?
- Où est enregistré le canal courant d’un utilisateur ?
  - Dans etat_serveur["utilisateurs"][pseudo]["canal"]
- Où sont les flux de sortie (wfile) associés à chaque client ?
  - Dans etat_serveur["utilisateurs"][pseudo]["wfile"]
### 3. Qui peut planter ?
- Que se passe-t-il si un client quitte sans envoyer /quit ?
  - Le serveur détecte la déconnexion dans la boucle handle() quand readline() retourne une ligne vide
  - Le nettoyage est effectué dans le bloc finally de handle()
- Qu’arrive-t-il si un write() échoue ? Est-ce détecté ?
  - Les write() sont dans des blocs try/except qui ignorent silencieusement les erreurs (except: continue)
  - L'erreur est détectée mais pas traitée proprement
- Est-ce qu’un canal vide est supprimé ?
  - Non, les canaux vides restent dans etat_serveur["canaux"] avec une liste vide
