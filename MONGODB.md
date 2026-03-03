# 🍃 MongoDB : Base de Données NoSQL Orientée Document

MongoDB est une base de données distribuée, NoSQL, qui stocke les données dans des documents flexibles de type JSON (BSON).

---

## 🏗️ Concepts de Base vs Relationnel

| Concept MongoDB | Équivalent SQL | Notes |
| :--- | :--- | :--- |
| **Document** | Ligne (Row) | Unité de base, stockée en BSON (Binary JSON). |
| **Collection** | Table | Groupe de documents, généralement sans schéma rigide. |
| **Field** | Colonne (Column) | Paire Clé/Valeur au sein d'un document. |
| **_id** | Clé Primaire | Identifiant unique généré automatiquement (ObjectId). |

---

## 💡 Pourquoi choisir MongoDB ? (Arguments Tech Lead)

* **Schéma Flexible (Dynamic Schema)** : Idéal pour les données dont la structure évolue (ex: attributs produits variés chez ADEO).
* **Scalabilité Horizontale** : Support natif du **Sharding** pour répartir les données sur plusieurs clusters.
* **Haute Disponibilité** : Mécanisme de **Replica Sets** avec basculement automatique (Failover).
* **Performance en Lecture** : Très rapide pour les requêtes simples grâce à l'indexation et au stockage de données liées dans un seul document (évite les JOIN coûteux).

---

## ⚙️ Modélisation : Embedding vs Referencing

C'est la question d'architecture clé en entretien :

1.  **Embedding (Dénormalisation)** : Inclure les données liées directement dans le document parent.
    * *Usage* : Données qui appartiennent exclusivement au parent (ex: adresses d'un utilisateur).
    * *Avantage* : Une seule lecture disque pour récupérer tout l'objet.
2.  **Referencing (Normalisation)** : Stocker l'ID d'un document d'une autre collection.
    * *Usage* : Données partagées entre plusieurs entités ou listes qui croissent indéfiniment.

---

## ⚡ Indexation & Agrégation

* **Index** : Supporte les index simples, composés, géospatiaux et textuels.
* **Aggregation Framework** : Pipeline de transformation de données (`$match`, `$group`, `$sort`, `$project`) extrêmement puissant, équivalent au `GROUP BY` et aux fonctions analytiques SQL.

---

## 🛠️ Intégration Java 25 / Spring Boot 3

* **Spring Data MongoDB** : Utilisation de `MongoRepository` pour une abstraction fluide.
* **Transactions Multi-documents** : Depuis la version 4.0, MongoDB supporte les transactions ACID sur plusieurs collections.
* **Document immuable** : Parfaitement compatible avec les **Java Records** pour la lecture de données.