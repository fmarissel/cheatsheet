# 📑 Apache Avro & Évolution des Schémas

Apache Avro est un système de sérialisation de données compact et binaire. Sa force réside dans la séparation du schéma (JSON) et des données (Binaire), permettant une évolution fluide des contrats d'interface.

---

## 🏗️ Le rôle du Schema Registry
Pour que Kafka et Avro fonctionnent ensemble, on utilise un **Schema Registry** (Confluent ou Karapace) :
1. Le **Producer** envoie le schéma au Registry et reçoit un ID.
2. Le **Producer** envoie le message sur Kafka avec l'ID du schéma en préfixe.
3. Le **Consumer** lit l'ID, récupère le schéma correspondant depuis le Registry et désérialise le message.



---

## 🔄 Types de Compatibilité

Le Schema Registry intercepte les nouveaux schémas et vérifie s'ils respectent la règle de compatibilité définie.

| Mode | Description | Règle d'or |
| :--- | :--- | :--- |
| **BACKWARD** | Un nouveau schéma peut lire les données écrites avec l'ancien. | **Supprimer** des champs ou ajouter des champs **optionnels** (avec valeur par défaut). |
| **FORWARD** | L'ancien schéma peut lire les données écrites avec le nouveau. | **Ajouter** des champs ou supprimer des champs **optionnels**. |
| **FULL** | Compatibilité dans les deux sens (Backward + Forward). | Modification uniquement sur les champs avec **valeurs par défaut**. |
| **NONE** | Aucune vérification. | À éviter en production (risque de rupture du contrat). |

---

## 🛠️ Stratégies d'Évolution (Best Practices)

1. **Toujours définir des valeurs par défaut** : C'est la règle n°1 pour permettre la compatibilité `FULL`. Sans `default`, toute suppression ou ajout devient une "breaking change".
2. **Éviter les Enums** : Les Enums Avro sont rigides. Si vous ajoutez un symbole, les anciens consommateurs ne sauront pas le gérer. Préférez des `String`.
3. **Nommage logique** : Utilisez des namespaces clairs pour vos schémas afin d'éviter les collisions dans le Registry.

---

## 🚀 Pourquoi Avro vs JSON ?
* **Performance** : Format binaire beaucoup plus léger que le texte JSON (gain de bande passante sur Kafka).
* **Robustesse** : Le schéma est un contrat fort. On ne peut pas envoyer "n'importe quoi" dans le topic.
* **Typage** : Supporte les types complexes, les records imbriqués et la documentation directement dans le schéma.