# 📦 Pattern Transactional Outbox

Le pattern **Outbox** résout le problème de l'atomicité entre une mise à jour de base de données et la publication d'un événement dans un message broker (comme Kafka).

---

## 🚩 Le Problème : Le "Dual Write"
Dans un microservice, si vous mettez à jour votre base de données et envoyez ensuite un message à Kafka, une panne peut survenir entre les deux actions :
* La DB est mise à jour, mais le message n'est jamais envoyé (perte de données).
* Le message est envoyé, mais la transaction DB échoue au dernier moment (incohérence).

---

## 🛠️ La Solution : La table Outbox
L'idée est de ne pas envoyer le message directement à Kafka depuis l'application, mais de l'écrire dans la même base de données que les données métier.

1. **Transaction atomique** : L'application met à jour ses tables métier ET insère le message dans une table technique nommée `OUTBOX` au sein de la même transaction SQL.
2. **Relais (Message Relay)** : Un processus séparé lit la table `OUTBOX` et publie les messages dans Kafka.

---

## 🔄 Stratégies de Relais

### 1. Change Data Capture (CDC) - *La méthode recommandée*
Un outil comme **Debezium** surveille les logs de la base de données (WAL pour PostgreSQL, binlog pour MySQL).
* **Avantages** : Très faible latence, aucun impact sur les performances de l'application, garantit l'ordre des événements.
* **Stack type** : Java 21 / Spring Boot 3 + PostgreSQL + Debezium + Kafka.

### 2. Polling (Balayage)
Un thread applicatif interroge périodiquement la table `OUTBOX` pour envoyer les nouveaux messages.
* **Inconvénients** : Moins performant (latence liée au polling), risque de surcharge de la DB à grande échelle.

---

## ✅ Avantages du Pattern
* **Garantie "At-least-once"** : Aucun message n'est perdu si la transaction DB réussit.
* **Cohérence forte** : L'état interne du service et les événements publiés sont toujours synchronisés.
* **Découplage** : L'application n'est pas bloquée si Kafka est temporairement indisponible.

---

## ⚠️ Points d'attention (Tech Lead Focus)
* **Idempotence des Consumers** : Le pattern garantit la livraison, mais peut générer des doublons en cas de retry du relais. Les consommateurs doivent être prêts.
* **Nettoyage** : Il faut prévoir un mécanisme pour purger les messages traités de la table `OUTBOX` afin d'éviter qu'elle ne grossisse indéfiniment.