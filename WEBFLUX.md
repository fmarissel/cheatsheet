# ⚡ Programmation Réactive avec Spring WebFlux

La programmation réactive est un paradigme orienté vers les flux de données et la propagation du changement. Elle est particulièrement utile pour les systèmes hautement concurrents.

---

## 🏗️ Modèle de Thread : MVC vs WebFlux

### Spring MVC (Servlet Stack)
* **Modèle** : One Thread Per Request (Bloquant).
* **Limite** : Chaque connexion occupe un thread. En cas de forte charge ou d'IO lents (appels API, DB), les threads s'épuisent vite (Thread Starvation).

### Spring WebFlux (Reactive Stack)
* **Modèle** : Event Loop (Non-bloquant).
* **Fonctionnement** : Un petit nombre de threads gère un grand nombre de connexions de manière asynchrone via une boucle d'événements (Netty).
* **Usage** : Idéal pour les applications avec beaucoup d'IO (Microservices, Passerelles API).



---

## 🧩 Concepts Clés : Project Reactor

WebFlux s'appuie sur **Project Reactor**, qui implémente la spécification *Reactive Streams*.

* **Mono<T>** : Un flux qui émet **0 ou 1** élément (ex: récupérer un utilisateur par ID).
* **Flux<T>** : Un flux qui émet **0 à N** éléments (ex: une liste de prix, un stream Kafka).
* **Backpressure** : Mécanisme permettant au consommateur de signaler au producteur qu'il reçoit trop de données, évitant ainsi de submerger le système.

---

## ⚙️ Les 4 règles d'or du Réactif

1. **Don't Block** : Ne jamais utiliser d'appels bloquants (JDBC classique, `Thread.sleep()`) dans un flux réactif, sous peine de bloquer l'Event Loop. Utiliser des drivers réactifs (R2DBC, WebClient).
2. **Lazy by design** : Rien ne se passe tant que vous ne souscrivez pas (`.subscribe()`). En WebFlux, c'est le framework qui gère la souscription finale.
3. **Immuabilité** : Les opérateurs (`map`, `flatMap`, `filter`) retournent toujours un nouveau flux.
4. **Gestion d'erreurs** : Les erreurs sont traitées comme des signaux dans le flux (`onErrorResume`, `onErrorReturn`).

---

## 🛠️ Stack Technique Réactive

* **Serveur** : Netty (par défaut).
* **Client HTTP** : `WebClient` (remplace `RestTemplate`).
* **Base de données** : **R2DBC** (pour le SQL réactif) ou drivers NoSQL natifs (MongoDB, Cassandra, Redis).
* **Sécurité** : Spring Security Reactive.

---

## 🚀 Pourquoi l'utiliser ? (Argumentaire Tech Lead)
* **Scalabilité** : Gérer des milliers de requêtes simultanées avec très peu de mémoire.
* **Résilience** : Meilleure gestion des timeouts et des retries via les opérateurs fluides de Reactor.
* **Streaming** : Capacité à envoyer des données au compte-gouttes (Server-Sent Events) au client.