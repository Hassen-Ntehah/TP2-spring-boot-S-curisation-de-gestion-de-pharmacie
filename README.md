# TP 2 : Sécurisation de gestion de pharmacie

**Module :** Spring Boot - MicroServices  
**Professeur :** Youness IDRISSI KHAMLICHI (ENSA Fès)  
**Projet :** `pharma_security`

---

## 🎯 Objectif
Mettre en œuvre la sécurité d'une application web (**authentification + autorisation**) en utilisant :
- **Spring Boot 3**
- **Spring Security 6** (BCrypt, DaoAuthenticationProvider, UserDetailsService, Form Login, RBAC)
- **Spring Data JPA** & **MySQL**
- **Thymeleaf** & **thymeleaf-extras-springsecurity6**
- **Bootstrap 5 & Icons** pour une interface moderne et responsive

---

## 👥 Matrice des Rôles & Droits d'Accès

| Rôle | Accès Autorisé | Pages Accessibles |
| :--- | :--- | :--- |
| **ADMIN** | Gérer les utilisateurs, les médicaments, les stocks (Accès complet) | `/admin`, `/medicaments`, `/ventes`, `/home` |
| **PHARMACIEN** | Consulter et modifier les stocks, vendre des médicaments | `/medicaments`, `/ventes`, `/home` |
| **CAISSIER** | Enregistrer les ventes uniquement | `/ventes`, `/home` |
| **CLIENT** | Consulter les médicaments disponibles (lecture seule) | `/medicaments` (filtré dispo), `/home` |

---

## 🧪 Scénarios de Test du TP

| Utilisateur | Mot de passe | Rôle | Droits testés |
| :--- | :--- | :--- | :--- |
| `admin` | `admin123` | **ADMIN** | Accès à tout le système : CRUD médicaments, ajustement stocks, gestion des utilisateurs. |
| `pharma` | `pharma123` | **PHARMACIEN** | Consultation médicaments, ajustement des stocks en rayon, validation de vente. Accès `/admin` bloqué (403). |
| `caisse` | `caisse123` | **CAISSIER** | Enregistrement des ventes et consultation de l'historique des tickets de caisse. Accès `/admin` bloqué (403). |
| `client` | `client123` | **CLIENT** | Consultation en lecture seule des médicaments en stock. Accès `/ventes` et `/admin` bloqués (403). |

> 💡 *Sur la page `/login`, des badges de remplissage rapide sont intégrés pour charger automatiquement les identifiants en un clic.*

---

## 📂 Structure du Projet

```text
pharma_security/
├── pom.xml
├── mvnw / mvnw.cmd
└── src/
    ├── main/
    │   ├── java/ma/ensaf/
    │   │   ├── PharmaSecurityApplication.java
    │   │   ├── config/
    │   │   │   └── SecurityConfig.java
    │   │   ├── controller/
    │   │   │   ├── LoginController.java
    │   │   │   ├── AdminController.java
    │   │   │   ├── MedicamentController.java
    │   │   │   └── VenteController.java
    │   │   ├── model/
    │   │   │   ├── User.java
    │   │   │   ├── Role.java
    │   │   │   ├── Medicament.java
    │   │   │   └── Vente.java
    │   │   ├── repository/
    │   │   │   ├── UserRepository.java
    │   │   │   ├── RoleRepository.java
    │   │   │   ├── MedicamentRepository.java
    │   │   │   └── VenteRepository.java
    │   │   └── service/
    │   │       ├── CustomUserDetailsService.java
    │   │       └── InitData.java
    │   └── resources/
    │       ├── application.properties
    │       └── templates/
    │           ├── login.html
    │           ├── home.html
    │           ├── admin.html
    │           ├── medicaments.html
    │           ├── ventes.html
    │           ├── access-denied.html
    │           └── fragments/
    │               └── navbar.html
    └── test/
        └── java/ma/ensaf/
            └── PharmaSecurityApplicationTests.java
```

---

## 🚀 Démarrage Rapide

### 1. Base de Données MySQL
Assurez-vous qu'un serveur MySQL est démarré sur le port `3306` (ou via Docker) :
```bash
docker run -d --name mysql-pharma -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -e MYSQL_DATABASE=pharma_security mysql:8
```

### 2. Configuration (`src/main/resources/application.properties`)
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/pharma_security?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
```

### 3. Compilation et Exécution des Tests
```bash
./mvnw clean test
```

### 4. Lancement de l'Application
```bash
./mvnw spring-boot:run
```
L'application est accessible sur : **http://localhost:8080**
