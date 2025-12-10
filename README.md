#  SmartHouse - Application Smart Home

Application complète de gestion de maison intelligente (Smart Home) développée avec Spring Boot, Angular et Docker.

##  Description du projet

**SmartHouse** est une application web permettant de gérer une maison intelligente avec les fonctionnalités suivantes :

-  Gestion des appareils (lampes, capteurs, objets connectés, etc.)
- Gestion des catégories d'appareils
-  Ajout de photos pour les appareils
- Activation / désactivation d'un appareil
- Interface web moderne et responsive
-  API REST complète
-  Base de données MySQL persistante

##  Architecture du projet

```
TP24-Microservices-main/
│
├── Smart_Home_back/          # Backend Spring Boot
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/example/appareil/
│   │   │   │       ├── config/          # Configuration CORS
│   │   │   │       ├── cotroller/       # Contrôleurs REST
│   │   │   │       ├── entity/          # Entités JPA
│   │   │   │       ├── repository/      # Repositories Spring Data
│   │   │   │       └── service/         # Services métier
│   │   │   └── resources/
│   │   │       └── application.properties
│   │   └── test/                         # Tests unitaires
│   ├── pom.xml
│   └── Dockerfile
│
├── smartHome-front/          # Frontend Angular
│   ├── src/
│   │   ├── app/
│   │   │   ├── controller/             # Services et modèles
│   │   │   ├── modules/                 # Composants Angular
│   │   │   └── app.module.ts
│   │   └── environments/                # Configuration environnement
│   ├── package.json
│   ├── angular.json
│   ├── Dockerfile
│   └── nginx.conf                        # Configuration Nginx
│
├── docker-compose.yml        # Orchestration Docker
└── README.md                 # Documentation
```

## Technologies utilisées

| Composant | Technologie |
|-----------|-------------|
| **Backend** | Java 17, Spring Boot 3.1.5, Spring Data JPA, Hibernate |
| **Frontend** | Angular, TypeScript, PrimeNG, Tailwind CSS |
| **Base de données** | MySQL 8.0 |
| **Interface DB** | phpMyAdmin |
| **Orchestration** | Docker & Docker Compose |
| **Build Backend** | Maven 3.9.6 |
| **Build Frontend** | Node.js 14, Angular CLI |
| **Web Server** | Nginx (Alpine) |

##  Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- [Docker](https://www.docker.com/get-started) (version 20.10 ou supérieure)
- [Docker Compose](https://docs.docker.com/compose/install/) (version 2.0 ou supérieure)

> **Note :** Aucune installation de Java, Node.js ou MySQL n'est nécessaire, tout est containerisé !

##  Installation et lancement

### 1. Cloner le projet

```bash
git clone <url-du-repo>
cd TP24-Microservices-main
```

### 2. Lancer l'application avec Docker

```bash
# Construire et lancer tous les services
docker-compose up --build -d

# Vérifier l'état des conteneurs
docker-compose ps

# Voir les logs
docker-compose logs -f
```

### 3. Arrêter l'application

```bash
# Arrêter les conteneurs
docker-compose down

# Arrêter et supprimer les volumes (⚠️ supprime les données)
docker-compose down -v
```

### 4. Rebuild complet

```bash
# Reconstruire les images
docker-compose build --no-cache

# Puis relancer
docker-compose up -d
```

## 🌐 Accès aux services

Une fois les conteneurs en cours d'exécution, vous pouvez accéder aux services suivants :

| Service | URL | Description |
|---------|-----|-------------|
| **Frontend Angular** | [http://localhost](http://localhost) | Interface utilisateur principale |
| **Backend API** | [http://localhost:8085](http://localhost:8085) | API REST Spring Boot |
| **phpMyAdmin** | [http://localhost:8081](http://localhost:8081) | Interface d'administration MySQL |
| **MySQL** | `localhost:3307` | Base de données (accès direct) |
<img width="1755" height="848" alt="image" src="https://github.com/user-attachments/assets/87a20c7e-85f1-4ec9-94a0-513c9618246b" />


### Identifiants par défaut

- **MySQL / phpMyAdmin :**
  - Utilisateur : `root`
  - Mot de passe : ``
  - Base de données : `smart_house`

## 🔧 Configuration

### Variables d'environnement

Les configurations sont définies dans `docker-compose.yml` et peuvent être modifiées selon vos besoins :

```yaml
environment:
  SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/smart_house?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
  SPRING_DATASOURCE_USERNAME: root
  SPRING_DATASOURCE_PASSWORD: root
```

### Ports

- **Frontend** : Port `80` (http://localhost)
- **Backend** : Port `8085` (http://localhost:8085)
- **MySQL** : Port `3307` (pour éviter les conflits avec WAMP/XAMPP)
- **phpMyAdmin** : Port `8081` (http://localhost:8081)

## 📡 API REST

### Endpoints disponibles

#### Appareils

- `GET /api/controller/appareil/` - Liste tous les appareils
- `GET /api/controller/appareil/id/{id}` - Récupère un appareil par ID
- `POST /api/controller/appareil/` - Crée un nouvel appareil
- `PUT /api/controller/appareil/id/{id}` - Met à jour un appareil
- `DELETE /api/controller/appareil/id/{id}` - Supprime un appareil
- `GET /api/controller/appareil/update/state/{state}` - Met à jour l'état de tous les appareils

#### Catégories

- `GET /api/controller/categorie/` - Liste toutes les catégories
- `GET /api/controller/categorie/id/{id}` - Récupère une catégorie par ID
- `POST /api/controller/categorie/` - Crée une nouvelle catégorie
- `PUT /api/controller/categorie/id/{id}` - Met à jour une catégorie
- `DELETE /api/controller/categorie/id/{id}` - Supprime une catégorie

### Exemple de requête

```bash
# Récupérer tous les appareils
curl http://localhost:8085/api/controller/appareil/

# Créer une catégorie
curl -X POST http://localhost:8085/api/controller/categorie/ \
  -H "Content-Type: application/json" \
  -d '{"label": "Lumières"}'
```

## 🧪 Tests

### Lancer les tests du backend

```bash
# Dans le conteneur Docker
docker-compose exec backend mvn test

# Ou créer un conteneur temporaire
docker-compose run --rm backend mvn test
```

### Tests unitaires

Les tests sont situés dans `Smart_Home_back/src/test/java/`

## 📝 Utilisation de l'application

### 1. Ajouter une catégorie

1. Accédez à l'application : http://localhost
2. Naviguez vers **Categories**
3. Cliquez sur **Add Category**
4. Entrez le label de la catégorie
5. Cliquez sur **Save**

### 2. Ajouter un appareil

1. Naviguez vers **Devices**
2. Cliquez sur **Add Device**
3. Remplissez le formulaire :
   - Label
   - Description
   - Photo (optionnelle)
   - Catégorie
4. Cliquez sur **Save**

### 3. Activer/Désactiver un appareil

- Utilisez le switch à côté de chaque appareil pour changer son état
- Ou utilisez le bouton global pour activer/désactiver tous les appareils

## 🐳 Structure Docker

### Services Docker

1. **mysql** : Base de données MySQL 8.0
2. **backend** : Application Spring Boot
3. **frontend** : Application Angular servie par Nginx
4. **phpmyadmin** : Interface d'administration MySQL

### Volumes

- `mysql_data` : Persistance des données MySQL

### Healthchecks

Le service MySQL inclut un healthcheck pour s'assurer que la base de données est prête avant de démarrer le backend.

## 🔒 Configuration CORS

L'application inclut une configuration CORS globale permettant les requêtes depuis le frontend. La configuration se trouve dans `Smart_Home_back/src/main/java/com/example/appareil/config/CorsConfig.java`.

## 🛠️ Développement

### Modifier le backend

1. Modifiez les fichiers dans `Smart_Home_back/src/`
2. Rebuild l'image : `docker-compose build backend`
3. Redémarrez : `docker-compose up -d backend`

### Modifier le frontend

1. Modifiez les fichiers dans `smartHome-front/src/`
2. Rebuild l'image : `docker-compose build frontend`
3. Redémarrez : `docker-compose up -d frontend`

### Accéder aux logs

```bash
# Tous les services
docker-compose logs -f

# Un service spécifique
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f mysql
```

## 📊 Base de données

### Schéma

- **Table `categorie`** : Catégories d'appareils
  - `id` (Long, Primary Key)
  - `label` (String)

- **Table `appareil`** : Appareils
  - `id` (Long, Primary Key)
  - `label` (String)
  - `description` (String)
  - `state` (Boolean)
  - `photo` (LONGTEXT)
  - `categorie_id` (Foreign Key vers `categorie`)

### Accès via phpMyAdmin

1. Accédez à http://localhost:8081
2. Connectez-vous avec :
   - Serveur : `mysql`
   - Utilisateur : `root`
   - Mot de passe : `root`

## ⚠️ Dépannage

### Le backend ne démarre pas

- Vérifiez que MySQL est bien démarré : `docker-compose ps`
- Consultez les logs : `docker-compose logs backend`
- Vérifiez la configuration dans `application.properties`

### Le frontend ne se connecte pas au backend

- Vérifiez que le backend est accessible : http://localhost:8085
- Vérifiez la configuration CORS
- Consultez la console du navigateur (F12)

### Erreur de connexion à la base de données

- Vérifiez que MySQL est en cours d'exécution
- Vérifiez les variables d'environnement dans `docker-compose.yml`
- Vérifiez que le nom de la base de données correspond (`smart_house`)
<img width="1102" height="244" alt="image" src="https://github.com/user-attachments/assets/ec17fb82-cad4-43ce-a71c-28e705f2ff04" />

### Ports déjà utilisés

Si un port est déjà utilisé, modifiez-le dans `docker-compose.yml` :

```yaml
ports:
  - "8080:80"  # Changez 8080 par un autre port disponible
```

## 📄 Licence

Ce projet est un projet éducatif réalisé dans le cadre d'un TP sur les microservices.

## 👥 Auteur

**Projet réalisé par jamila dabachine**

Encadré dans le cadre des projets Microservices & SmartHome.

## 🙏 Remerciements

- Spring Boot pour le framework backend
- Angular pour le framework frontend
- PrimeNG pour les composants UI
- Docker pour la containerisation

---

