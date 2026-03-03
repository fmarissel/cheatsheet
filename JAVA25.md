# ☕ Java 25 LTS : La nouvelle référence

Java 25 est la version Long Term Support qui succède à Java 21. Elle stabilise plusieurs projets majeurs qui transforment la façon dont nous écrivons nos microservices.

---

## 🚀 Fonctionnalités Clés de Java 25

### 1. Scoped Values (Standard)
Remplace les `ThreadLocal` pour le partage de données immuables.
* **Intérêt** : Indispensable avec les millions de **Virtual Threads** pour passer des contextes (User ID, Trace ID) sans consommer de mémoire inutile.

### 2. Structured Concurrency (Standard)
Permet de traiter plusieurs tâches asynchrones comme une seule unité.
* **Principe** : Si une sous-tâche échoue, toutes les autres sont annulées automatiquement. Fini les threads "orphelins" qui continuent de tourner dans le vide.

### 3. Flexible Constructor Bodies
Permet d'exécuter du code (validation, calculs) *avant* d'appeler `super()` dans un constructeur. Cela offre une bien meilleure flexibilité pour l'initialisation des classes complexes.

### 4. Stream Gatherers
Ajoute des capacités de traitement personnalisées aux Streams (fenêtrage, regroupement complexe), rendant l'API Stream encore plus puissante pour le traitement de flux de données (Kafka/PostgreSQL).

---

## 🏗️ Pourquoi passer de Java 21 à Java 25 ? (Argument Tech Lead)

| Amélioration | Impact Business / Technique |
| :--- | :--- |
| **Project Loom (Finalisé)** | Scalabilité massive sur Kubernetes (GKE) avec une consommation RAM réduite de 40% par rapport au modèle réactif pur. |
| **Project Panama** | Accès ultra-rapide aux bibliothèques hors-JVM (IA, calcul vectoriel), utile pour les calculs de prix complexes. |
| **Maintenance** | Support long terme garanti jusqu'en 2030+, assurant la pérennité des développements actuels chez Decathlon. |

---

## 🛠️ Rappel : Toujours d'actualité depuis Java 21
* **Virtual Threads** : Utilisation intensive pour les serveurs HTTP (Tomcat/Jetty) et les listeners Kafka.
* **Pattern Matching** : Utilisation systématique avec les **Records** pour un code plus lisible et moins sujet aux erreurs.