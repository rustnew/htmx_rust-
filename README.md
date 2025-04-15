### **Description Globale du Projet**  

Ce projet est une **application de gestion de tâches (Todo List)** en temps réel, développée en **Rust** avec le framework web **Axum**. Il combine :  
- **Un backend performant** (API REST + SSE pour les updates live).  
- **Un frontend léger** (HTMX pour l'interactivité sans JavaScript complexe).  
- **Une base de données PostgreSQL** pour le stockage persistant.  

L'application permet :  
✅ **Ajouter/supprimer des tâches** avec mise à jour immédiate pour tous les utilisateurs connectés.  
✅ **Afficher une liste dynamique** sans rechargement de page (via SSE).  
✅ **Servir des assets statiques** (CSS) pour une UI propre.  

---

### **🛠️ Structure du Code**  

#### **1. Backend (Axum)**
- **Routes principales** :  
  - `GET /` → Page d’accueil (`home()`).  
  - `GET /todos` → Récupère toutes les tâches (`fetch_todos()`).  
  - `POST /todos` → Crée une tâche (`create_todo()`).  
  - `DELETE /todos/:id` → Supprime une tâche (`delete_todo()`).  
  - `GET /stream` → Flux SSE pour les updates temps réel (`handle_stream()`).  
  - `GET /styles.css` → CSS statique (`styles()`).  

- **Gestion des données** :  
  - **SQLx** pour les requêtes PostgreSQL.  
  - **Modèles** (`Todo`, `TodoNew`, `TodoUpdate`) pour structurer les données.  

- **Temps réel** :  
  - **`BroadcastStream`** (Tokio) pour notifier les clients des changements.  
  - **Server-Sent Events (SSE)** pour pousser les updates au frontend.  

#### **2. Frontend (HTMX + Templates)**
- **Templates HTML** :  
  - Intégration directe avec Axum (ex: `templates::TodoNewTemplate`).  
  - Utilisation de **HTMX** pour les requêtes AJAX (`hx-post`, `hx-delete`).  
- **CSS** : Fichier statique servi via `/styles.css`.  

#### **3. Architecture**
- **État partagé** (`AppState`) :  
  - Contient le pool de connexions à la DB (`db`) et le canal de broadcast (`tx`).  
- **Gestion d’erreurs** :  
  - Centralisée via `ApiError` (erreurs SQLx, Axum, etc.).  

---

### **🚀 Comment Lancer le Projet**  

#### **Prérequis**  
1. **Rust** : Installé via [rustup](https://rustup.rs/).  
2. **PostgreSQL** : Serveur local ou distant (avec une DB configurée).  
3. **Variables d’environnement** : Créez un fichier `.env` à la racine :  
   ```env
   DATABASE_URL=postgres://user:password@localhost/db_name
   ```

#### **Étapes**  
1. **Cloner le dépôt** (si applicable) :  
   ```bash
   git clone <repo_url>
   cd <project_dir>
   ```

2. **Installer les dépendances** :  
   ```bash
   cargo build
   ```

3. **Appliquer les migrations SQLx** :  
   ```bash
   sqlx migrate run
   ```

4. **Lancer le serveur** :  
   ```bash
   cargo run
   ```
   - Le serveur démarre sur `http://localhost:3000`.  

5. **Accéder à l’application** :  
   - Ouvrez `http://localhost:3000` dans un navigateur.  

---

### **📌 Fonctionnement en Pratique**  
1. **Ajouter une tâche** :  
   - Le formulaire HTMX envoie une requête `POST /todos`.  
   - La tâche est sauvegardée en DB et broadcastée à tous les clients via SSE.  

2. **Supprimer une tâche** :  
   - Clic sur un bouton `hx-delete` → requête `DELETE /todos/:id`.  
   - Mise à jour immédiate de l’UI pour tous les utilisateurs.  

3. **Streaming temps réel** :  
   - La page `/stream` écoute les événements SSE et met à jour la liste automatiquement.  

---

### **🔧 Debugging**  
- **Logs** : Vérifiez les logs du serveur (`cargo run`).  
- **Erreurs SQLx** : Activez le logging avec `RUST_LOG=sqlx=info`.  
- **SSE** : Testez avec `curl http://localhost:3000/stream` pour voir les événements.  

---

### **🌍 Déploiement**  
- **Cloud** : Utilisez **Docker** + **AWS/Google Cloud** (avec image Rust + PostgreSQL).  
- **Render/Heroku** : Configurez le buildpack Rust et la DB.  

--- 

Un projet **minimaliste mais puissant** pour explorer Rust, Axum, et HTMX ensemble ! 🦀  

