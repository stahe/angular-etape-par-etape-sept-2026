# Introduction étape par étape au framework Angular

📖 **Lire le tutoriel : [https://stahe.github.io/angular-etape-par-etape-sept-2026/](https://stahe.github.io/angular-etape-par-etape-sept-2026/)**

Ce cours vous apprend à écrire une application web avec le framework [Angular](https://angular.dev) 22 : une application **à page unique** (SPA), dont les pages sont fabriquées **dans le navigateur**, à partir des données JSON d'un serveur.

Il fait suite au cours [Introduction étape par étape au framework web NestJS](https://stahe.github.io/nestjs-html-sept-2026/), dont il change le point de vue, et reprend le plan du cours [Introduction étape par étape au framework Vue.js](https://stahe.github.io/vuejs-etape-par-etape-sept-2026/) : même serveur, mêmes pages, écrites à la manière d'Angular.

| Cours NestJS | Cours Angular |
|---|---|
| le serveur fabrique les pages HTML (Handlebars) | le navigateur fabrique les pages (Angular) |
| le navigateur affiche ce qu'il reçoit | le serveur ne renvoie que du JSON |
| contrôleurs, vues, `res.render` | composants, routeur, services |
| gardes `JwtAuthGuard`, `RolesGuard` | gardes du routeur (le serveur garde les siens) |
| dictionnaires lus par le serveur | dictionnaires dans le client (ngx-translate) ; le serveur ne renvoie que des clés |
| message flash dans un cookie | message flash dans un service à signaux |

Les pages, elles, ne changent pas : ce sont celles de l'application **RdvMedecins** déjà présentée avec les autres frameworks.

## L'approche : de nombreux petits exemples, puis une étude de cas

Angular étant plus riche que Vue.js, le cours s'articule autour de **25 petits exemples**, chacun centré sur une notion. Ils forment un seul espace de travail Angular : une seule commande `npm install`, puis `npm start <exemple>` pour en lancer un.

| Chapitre | Contenu | Exemples |
|---|---|---|
| Premiers pas | un espace de travail Angular, les signaux (`signal`, `computed`, `effect`, `linkedSignal`), les templates (`@if`, `@for`, `@let`), les événements, `ngModel`, les formulaires à signaux, la validation, les formulaires réactifs, les pipes | 01–09 |
| Les composants | `input()`, `output()`, `model()`, projection de contenu, cycle de vie, services et injection de dépendances, directives, fenêtre de confirmation | 10–16 |
| Le routage | le routeur, paramètres, query, chargement différé, gardes de navigation | 17–18 |
| Observables et état partagé | RxJS (`debounceTime`, `switchMap`, `toSignal`), services à signaux, `localStorage` | 19–20 |
| Internationalisation | ngx-translate : paramètres, pluriels, dates, montants | 21 |
| Le serveur, une boîte noire | installation du serveur JSON, son API, 48 exemples `curl` | – |
| Dialoguer avec le serveur | `HttpClient`, proxy de `ng serve`, cookie `httpOnly`, intercepteur, couche d'accès à l'API, `httpResource`, erreurs du serveur attachées aux champs | 22–25 |

Chaque exemple est présenté avec son code complet, commenté ligne par ligne, et une copie d'écran de son exécution.

## Le serveur : une boîte noire

Le serveur est le serveur NestJS du cours précédent, dont les contrôleurs renvoient du **JSON** — le même que pour le client Vue.js. Le cours le traite comme une **boîte noire** : on l'installe, on étudie son API, on l'interroge avec `curl` — mais on n'a pas besoin de lire son code (fourni et commenté pour les curieux).

- toutes les erreurs ont la même forme : `{ "statusCode": 409, "cle": "ERRORS.LOGIN_TAKEN", "params": {...}, "champs": {...} }` — des **clés** de traduction, jamais de texte ;
- authentification par jeton JWT dans un cookie `httpOnly` / `sameSite=strict` : le code JavaScript du client ne voit jamais le jeton ;
- protection CSRF : cookie `sameSite`, et tout POST doit être en JSON ;
- un « mode test » du captcha pour pouvoir interroger l'API avec `curl`.

## L'étude de cas : le client Angular de RdvMedecins

Une application complète de **prise de rendez-vous dans un cabinet médical**, dont **tous** les fichiers (une cinquantaine) sont listés et commentés.

- **Angular moderne** : composants autonomes, signaux, détection des changements sans zone.js, formulaires à signaux avec contrôles personnalisés (`FormValueControl`), pages chargées à la demande.
- **Trois rôles** : `ADMIN` (gère les médecins et les clients), `DOCTOR` (prend et annule les rendez-vous), `USER` (le patient : réserve pour lui-même, gère son compte).
- **Confidentialité** : un patient ne reçoit jamais le nom des autres patients — le serveur ne l'envoie pas.
- **Tout l'état de la page dans l'URL** : `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` rouvre la fenêtre de réservation après un F5 ; les boutons Précédent / Suivant fonctionnent.
- **Validation par le serveur** : les formulaires affichent sous chaque champ les erreurs renvoyées par l'API ; verrou optimiste, homonymes, login déjà pris...
- **Session** : rétablie après un F5 (`GET /api/auth/moi`), expiration gérée en un seul endroit (un intercepteur HTTP, réponse 401).
- **Français / anglais**, titres d'onglet traduits, fenêtre de confirmation accessible au clavier, Bootstrap 5.
- **Déploiement** : le client compilé est servi par le serveur JSON lui-même (même origine, pas de CORS).

## Technologies

Angular 22.2 (composants autonomes, signaux, formulaires à signaux, zoneless) · TypeScript 6.0 · Angular CLI · RxJS 7.8 · ngx-translate 18 · Bootstrap 5.3 · côté serveur : NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Prérequis

- Des bases de JavaScript (ou TypeScript), de HTML et du protocole HTTP.
- Node.js 24 (ou 22.22.3 au moins), Visual Studio Code avec l'extension Angular Language Service, un serveur MySQL (par exemple Laragon sous Windows) pour le serveur JSON. Les instructions d'installation sont données dans les annexes du cours.

## Auteur

Ce cours, ses exemples, le serveur JSON et l'étude de cas ont été rédigés par **Claude**, l'IA d'[Anthropic](https://www.anthropic.com) (septembre 2026) à la demande de **Serge Tahé**
