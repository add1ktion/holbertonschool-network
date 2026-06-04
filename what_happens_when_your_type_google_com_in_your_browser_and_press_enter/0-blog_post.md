# Behind the Screens: What Happens When You Type https://www.google.com in Your Browser?

C'est la question incontournable, le grand classique des entretiens techniques pour les ingénieurs logiciel, les administrateurs systèmes et les profils DevOps. À première vue, l'action semble anodine : vous tapez une adresse, vous appuyez sur Entrée, et une fraction de seconde plus tard, la page d'accueil de Google s'affiche. 

Pourtant, derrière cette apparente simplicité se cache une chorégraphie complexe, orchestrée par une multitude de protocoles, de composants matériels et logiciels. Pour valider notre compréhension de la pile technique (*web stack*), décomposons chaque étape de ce voyage, du clavier jusqu'aux serveurs de production.

---

## 1. La boussole du Web : La requête DNS
Tout commence par une question d'identité. Les ordinateurs ne communiquent pas en utilisant des noms textuels comme `www.google.com`, mais via des adresses numériques uniques appelées **adresses IP** (ex: `142.250.190.46`). Le **DNS (Domain Name System)** fait office d'annuaire mondial de l'Internet.

Lorsque vous validez l'URL, votre navigateur initie une recherche hiérarchique :
* **Les Caches Locaux :** Le navigateur vérifie d'abord son propre historique récent, puis interroge le cache du système d'exploitation (OS).
* **Le Résolveur DNS :** Si l'adresse reste inconnue, la demande est envoyée au résolveur de votre fournisseur d'accès à Internet (FAI).
* **La hiérarchie racine :** Si besoin, le résolveur interroge les serveurs Racines (`.`), qui le redirigent vers les serveurs du domaine de premier niveau (TLD) (`.com`). En cascade, ces derniers pointent vers le **Serveur de Noms Autoritaire** de Google, qui renvoie enfin l'IP exacte.

---

## 2. Établir la poignée de main : Le protocole TCP/IP
Une fois l'adresse IP cible obtenue, le navigateur doit ouvrir un canal de communication fiable avec la machine distante. C'est le rôle du protocole **TCP (Transmission Control Protocol)**, couplé à **IP (Internet Protocol)**.

Pour garantir qu'aucun paquet de données ne soit perdu ou corrompu en chemin, la connexion s'établit via un mécanisme strict appelé la **triple poignée de main (TCP Three-Way Handshake)** :
* **SYN (Synchronize) :** Votre machine envoie un segment pour demander l'ouverture d'une session.
* **SYN-ACK (Synchronize-Acknowledge) :** Le serveur distant répond qu'il a bien reçu la demande et qu'il est prêt.
* **ACK (Acknowledge) :** Votre machine confirme la réception. La connexion bidirectionnelle est maintenant active.

---

## 3. Le filtrage de sécurité : Le Pare-feu (Firewall)
Avant même que les données ne traversent les réseaux profonds, chaque paquet passe par des contrôles de sécurité rigoureux. Le **Pare-feu** (présent aussi bien côté client que devant les infrastructures de Google) analyse les flux entrants et sortants.

En se basant sur des règles strictes de filtrage (ports autorisés, protocoles valides, signatures d'attaques), il agit comme un agent de sécurité. Si un paquet est suspect ou ne respecte pas la politique de sécurité, il est immédiatement rejeté pour protéger l'infrastructure contre les intrusions.

---

## 4. Le verrou de confiance : HTTPS et la négociation SSL/TLS
Parce que la saisie commence par `https://` et non `http://`, la session doit être chiffrée. Le protocole **HTTPS** superpose HTTP sur une couche de sécurité appelée **SSL/TLS (Secure Sockets Layer / Transport Layer Security)**.

Une seconde négociation s'engage alors : le serveur présente son **certificat SSL** (émis par une autorité de certification de confiance). Votre navigateur valide la clé publique du serveur et génère une clé de chiffrement symétrique unique pour cette session. À partir de cet instant, toutes les requêtes de recherche et données échangées deviennent totalement illisibles pour un éventuel espion sur le réseau.

---

## 5. Le chef d'orchestre : Le Répartiteur de charge (Load-Balancer)
Google reçoit plusieurs dizaines de milliers de requêtes par seconde. Un seul serveur s'effondrerait instantanément sous la charge. C'est ici qu'intervient le **Load-Balancer** (répartiteur de charge).

Placé en première ligne de l'infrastructure, ce composant matériel ou logiciel reçoit la requête sécurisée et choisit, selon un algorithme défini (comme le *Round Robin* ou le calcul des connexions minimales), vers quel serveur disponible la diriger. Il assure ainsi une haute disponibilité : si un serveur tombe en panne, le trafic est instantanément redirigé vers une machine saine sans interruption pour l'utilisateur.

---

## 6. Le premier point de contact : Le Serveur Web
La requête arrive enfin sur un **Serveur Web** (tel que Nginx, Apache ou les architectures internes de Google). Son rôle principal est de traiter les requêtes HTTP brutes.

Si la demande concerne un fichier statique (une image fixe, une feuille de style CSS), le serveur web peut la renvoyer directement. Mais pour une application dynamique complexe comme le moteur de recherche Google, le serveur web délègue le traitement à la couche suivante.

---

## 7. Le moteur logique : Le Serveur d'Application
C'est ici que réside la logique métier de l'application (le code écrit en Python, Go, C++, ou Java). Le **Serveur d'Application** prend en charge la requête, interprète vos paramètres (vos mots-clés de recherche, vos préférences de compte, etc.) et exécute les algorithmes nécessaires.

Pour construire une page sur mesure (comme vos suggestions personnalisées), il a besoin de données stockées à long terme. Il va donc formuler une requête vers la brique finale de la pile.

---

## 8. La mémoire du système : La Base de Données
Le serveur d'application interroge le système de gestion de **Bases de Données** (comme des clusters de bases de données distribuées à l'image de Google Spanner). C'est là que sont stockés l'index global du web, vos profils, et l'ensemble des configurations nécessaires. La base de données extrait l'information demandée et la renvoie au serveur d'application.

> **Le voyage de retour :** Une fois les données récupérées, le serveur d'application génère dynamiquement la page au format HTML, CSS et JavaScript. Le flux fait le chemin inverse : transmis au serveur web -> encapsulé par le load-balancer -> chiffré via le tunnel TLS -> validé par les pare-feu -> découpé en paquets TCP/IP -> et livré à votre navigateur. Ce dernier assemble le DOM, applique les styles et exécute les scripts pour afficher l'interface familière de Google.

---

*Projet réalisé dans le cadre du cursus System Engineering & DevOps - Holberton School.*

* **Dépôt GitHub :** `holbertonschool-network`  
* **Répertoire :** `what_happens_when_your_type_google_com_in_your_browser_and_press_enter`  
* **Fichier :** `0-blog_post`