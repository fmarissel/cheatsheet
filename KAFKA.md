# 🚀 Apache Kafka : Guide de Référence

Ce document synthétise les concepts fondamentaux d'Apache Kafka pour le développement d'architectures distribuées et réactives.

---

## 🏗️ Concepts Fondamentaux

Kafka est une plateforme de **streaming d'événements** distribuée basée sur un commit log immuable.

* **Producer** : Application qui envoie (publie) des messages vers des topics Kafka.
* **Consumer** : Application qui s'abonne à des topics pour lire et traiter les messages.
* **Broker** : Serveur Kafka qui stocke les données et sert les clients. Un cluster est composé de plusieurs brokers.
* **Topic** : Catégorie ou nom de flux de messages (équivalent à une table en SQL, mais sous forme de flux).
* **Partition** : Division d'un topic permettant le passage à l'échelle (scalability). L'ordre des messages n'est garanti qu'au sein d'une même partition.
* **Offset** : Identifiant séquentiel unique assigné à chaque message dans une partition.

---

## ⚙️ Mécanismes Avancés

### Consumer Groups
Permettent de répartir la charge de lecture. Chaque partition d'un topic est lue par un seul membre du groupe à la fois. Si un consumer tombe, Kafka rééquilibre (**rebalance**) les partitions vers les autres membres.

### Réplication & Haute Disponibilité
* **Replication Factor** : Nombre de copies d'une donnée à travers le cluster.
* **ISR (In-Sync Replicas)** : Liste des réplicas qui sont parfaitement à jour avec le "Leader" de la partition.

### Garanties de Livraison
1.  **At most once** : Les messages peuvent être perdus mais ne sont jamais redistribués.
2.  **At least once** : Les messages ne sont jamais perdus mais peuvent être redondants (nécessite de l'idempotence côté consumer).
3.  **Exactly once** : Traitement transactionnel garantissant que chaque message est traité une seule fois (via `processing.guarantee=exactly_once_v2`).

---

## 🛠️ Best Practices (Tech Lead Focus)

* **Idempotence** : Toujours concevoir les consumers pour qu'ils soient idempotents afin de gérer les rediffusions sans effets de bord.
* **Key Design** : Choisir une clé de message pertinente pour garantir que les événements liés (ex: les mises à jour d'un même `order_id`) arrivent dans la même partition et conservent leur ordre.
* **Schema Registry** : Utiliser un registre de schémas (Avro ou Protobuf) pour gérer l'évolution des contrats de données et éviter de casser les consommateurs.
* **Monitoring** : Surveiller le **Consumer Lag** (retard de lecture) via des outils comme Datadog ou Prometheus pour détecter les goulots d'étranglement.

---

## 🔄 Patterns d'Architecture

* **Event Sourcing** : Utiliser Kafka comme source de vérité absolue pour reconstruire l'état d'un système.
* **CQRS** : Séparer les commandes (écriture via Kafka) des requêtes (lecture via une base de données optimisée alimentée par Kafka).
* **Outbox Pattern** : Coupler une base de données transactionnelle et Kafka (souvent avec Debezium) pour garantir la publication d'un événement après une écriture en base.