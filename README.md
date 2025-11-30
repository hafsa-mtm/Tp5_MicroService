# TP 5 – Architecture Orientée Services et APIs  
## Application de gestion pédagogique (Microservices, Security, Eureka, OpenFeign, Config Server)

Ce repository contient une application basée sur une architecture microservices.  
Elle implémente l’authentification sécurisée, la découverte des services, la configuration centralisée, la communication inter-services via OpenFeign, et la gestion des étudiants et des filières.

---

## 1. Architecture Globale des Microservices

### 1. Configuration_Service (Spring Cloud Config Server)
- Fournit une configuration centralisée pour tous les microservices.
- Connecté à un dépôt Git contenant les fichiers de configuration.
- Enregistré comme Eureka Client.

### 2. Discovery_Service (Eureka Server)
- Service de découverte dynamique.
- Tous les microservices s’y enregistrent automatiquement.
- Permet au Gateway et aux clients Feign de localiser les services sans connaître leurs ports.

### 3. Gateway_Service (Spring Cloud Gateway)
- Point d’entrée unique dans l’architecture.
- Gère le routage dynamique vers les microservices enregistrés dans Eureka.
- Intègre Actuator pour le monitoring.

### 4. Security_Service
Service responsable de l’authentification et de la gestion des tokens JWT.  
Fonctionnalités :
- Gestion des utilisateurs (userInMemory).
- Génération et validation des tokens JWT.
- Sécurisation des endpoints des autres services.

### 5. MicroService Étudiant (Java)
- Fournit les opérations CRUD des étudiants.
- Communique avec le MicroService Filière pour obtenir les informations des filières associées (OpenFeign Client).
- Sécurisé via JWT fourni par le Security Service.

### 6. MicroService Filière (Flask Python)
- Gestion des filières académiques.
- Fournit les opérations CRUD sur les filières.
- Protégé par validation des tokens JWT (signés par le Security Service).
- Enregistré comme Eureka Client.

---

## 2. Technologies & Bibliothèques Utilisées

### Backend Java
- Java 17+
- Spring Boot 3+
- Spring MVC
- Spring Data JPA
- Hibernate
- MySQL

### Backend Python
- Python 3.8+
- Flask
- SQLAlchemy
- PyMySQL

### Microservices (Spring Cloud)
- Spring Cloud OpenFeign
- Spring Cloud Gateway
- Spring Cloud Config Server
- Spring Cloud Config Client
- Spring Cloud Netflix Eureka Server
- Spring Cloud Netflix Eureka Client
- Spring Boot Actuator

### Sécurité
- Spring Security
- OAuth2 Resource Server
- JWT (RS256)

### Outils
- Maven
- Lombok
- Swagger / OpenAPI
- Virtualenv (Python)

---

## 3. Fonctionnement Général

1. Le Config Service charge la configuration depuis un dépôt Git.
2. Chaque microservice récupère sa configuration via le Config Client.
3. Tous les microservices s’enregistrent dans Eureka Server.
4. Le Gateway route les requêtes vers les microservices appropriés.
5. Le Security Service génère et valide les tokens JWT signés avec une clé RSA privée.
6. Les microservices communiquent entre eux via OpenFeign (Java) ou via REST (Filière en Flask).
7. Actuator fournit des endpoints de monitoring.

---

## 4. Ordre de Démarrage Recommandé

1. Configuration_Service  
2. Discovery_Service (Eureka Server)  
3. Gateway_Service  
4. Security_Service  
5. Étudiant_Service (Java)  
6. Filière_Service (Flask Python)

---

## 5. Test avec Postman

- Pour obtenir un token :  
  POST `/login` sur le Security Service avec `username` et `password` en form-data.
- Copier le `Access_Token` reçu.
- Pour les requêtes vers les microservices (ex. Filière ou Étudiant), ajouter dans l’onglet **Authorization** → **Bearer Token** → coller le token.
- Les GET peuvent être testés sans token si configuré ainsi.

---
