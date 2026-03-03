# 🧠 Software Engineering Principles & Best Practices

Ce document regroupe les principes fondamentaux de conception logicielle.

---

## 🏗️ SOLID Principles
*Les 5 piliers de la conception orientée objet pour un code maintenable.*

| Principe | Description |
| :--- | :--- |
| **S**RP | **Single Responsibility Principle** : Une classe ne doit avoir qu'une seule raison de changer. |
| **O**CP | **Open/Closed Principle** : Ouvert à l'extension, fermé à la modification. |
| **L**SP | **Liskov Substitution Principle** : Les sous-classes doivent être substituables par leur classe de base. |
| **I**SP | **Interface Segregation Principle** : Préférer plusieurs interfaces spécifiques à une seule interface généraliste. |
| **D**IP | **Dependency Inversion Principle** : Dépendre des abstractions, pas des implémentations. |

---

## 💡 Pragmatic Design (DRY, KISS, YAGNI)
*Lutter contre la complexité et la duplication.*

* **DRY (Don't Repeat Yourself)** : Chaque fragment de connaissance doit avoir une représentation unique et non ambiguë dans le système.
* **KISS (Keep It Simple, Stupid)** : La simplicité doit être un objectif clé. Un code simple est plus facile à maintenir et à tester.
* **YAGNI (You Ain't Gonna Need It)** : Ne pas implémenter de fonctionnalités "au cas où". On ne développe que ce dont on a besoin maintenant.

---

## ⚡ Data & Transactions (ACID)
*Garantir la fiabilité et l'intégrité des données.*

* **Atomicity** : La transaction s'effectue entièrement ou pas du tout (tout ou rien).
* **Consistency** : La transaction fait passer le système d'un état valide à un autre état valide.
* **Isolation** : L'exécution simultanée de transactions doit donner le même résultat qu'une exécution séquentielle.
* **Durability** : Une fois validée, la transaction est enregistrée de façon permanente, même en cas de panne.

---

## 🧩 GRASP Patterns
*General Responsibility Assignment Software Patterns.*

* **Information Expert** : Assigner la responsabilité à la classe qui détient les informations nécessaires pour la remplir.
* **Creator** : Déterminer quelle classe est responsable de la création d'une nouvelle instance d'un objet.
* **Low Coupling** : Maintenir des dépendances faibles entre les modules pour faciliter les évolutions et les tests.
* **High Cohesion** : S'assurer que les responsabilités d'un module sont fortement liées et focalisées.

---

## 🛠️ Application Concrète (Tech Stack)
* **Java 21 & Spring Boot 3** : Utilisation des Records pour l'immuabilité et de l'injection de dépendances pour le découplage.
* **Architecture Hexagonale** : Isolation du Domain (Métier) des détails d'infrastructure (API, Base de données, Kafka).
* **Qualité Logicielle** : Validation systématique du comportement via des tests automatisés (TDD/BDD).
* **DevOps** : Automatisation des déploiements et monitoring pour garantir la stabilité en production.

---

> **Note :** Ce document sert de base pour les revues de code et le mentoring technique.