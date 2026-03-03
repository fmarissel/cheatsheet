# 🔄 Orchestration & Flux de Données : Pattern Saga

Dans une architecture microservices, une transaction métier peut s'étendre sur plusieurs services. Le pattern **Saga** permet de gérer la cohérence éventuelle (*Eventual Consistency*) sans utiliser de transactions distribuées (2PC), qui sont peu performantes.

---

## 🏗️ Fonctionnement
Une Saga est une séquence de transactions locales. Chaque transaction met à jour la base de données d'un service et publie un message/événement pour déclencher la suivante.

* **Transactions de compensation** : Si une étape échoue, la Saga doit exécuter des transactions d'annulation pour les étapes précédentes afin de revenir à un état cohérent.



---

## 🛤️ Les deux approches

### 1. Chorégraphie (Choreography)
Chaque service publie et écoute des événements (souvent via **Kafka**). Il n'y a pas de chef d'orchestre central.
* **Avantages** : Couplage faible, simple à mettre en place pour de petites Sagas.
* **Inconvénients** : Difficile à monitorer, risque de dépendances cycliques si la logique devient complexe.

### 2. Orchestration
Un service central (l'Orchestrateur) décide de l'ordre des transactions et appelle les services concernés.
* **Avantages** : Logique métier centralisée, facilité de monitoring, évite le "plat de spaghettis" d'événements.
* **Inconvénients** : Risque de centraliser trop de logique métier (Smart Endpoint / Dumb Pipes).

---

## ⚡ Stratégies de Flux de Données

### Outbox Pattern
Pour garantir qu'un message est envoyé dans Kafka **uniquement** si la transaction en base de données réussit.
* On écrit le message dans une table `OUTBOX` de la même DB que le métier (dans la même transaction).
* Un outil comme **Debezium** lit cette table et pousse le message vers Kafka.

### Dead Letter Topic (DLT)
Lorsqu'un message ne peut pas être traité après plusieurs tentatives (erreur technique), il est envoyé dans un topic spécifique (DLT) pour analyse humaine ou re-traitement ultérieur, évitant de bloquer le flux principal.
