
# Projet Flask Dockerisé

Ce projet met en place une application Flask de base, encapsulée dans un conteneur Docker. Il permet de déployer facilement l'application, que ce soit pour des tests en local ou pour une utilisation en production. Ce guide fournit toutes les étapes pour configurer, exécuter et tester l'application dans un environnement Docker.

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants installés :

    - Docker
    - Python 3.8+ (pour un développement local)

## Organisation du projet

```plaintext
.
├── app.py               # Code principal de l'application Flask
├── Dockerfile           # Fichier Docker pour créer l'image de l'application
├── requirements.txt     # Liste des bibliothèques Python nécessaires
├── .dockerignore        # Fichier pour ignorer des dossiers/fichiers dans Docker
└── README.md            # Guide du projet
```

## Mise en place de l'environnement

### 1. Environnement virtuel (optionnel, pour développement local)

Pour travailler localement, il est recommandé d'utiliser un environnement virtuel Python :

```bash
python3 -m venv .venv
source .venv/bin/activate  # Sur macOS/Linux
.venv\Scripts\activate     # Sur Windows
```

Ensuite, installez les dépendances avec :

```bash
pip install -r requirements.txt
```

### 2. Configuration Docker

Le projet est conçu pour s'exécuter dans un conteneur Docker, ce qui assure une portabilité et une cohérence entre différents environnements.

## Détails des fichiers principaux

### `app.py`

Ce fichier contient le code de l'application Flask. Elle initialise un serveur simple et expose une API accessible via la route /, qui retourne un message JSON.

Exemple du code :

```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/')
def home():
    return jsonify({"Yo les cool kyds!"})

if __name__ == '__main__':
    print("L'application Flask est en cours d'exécution.")
    print("Accédez à l'application sur : http://localhost:8000")
    app.run(host='0.0.0.0', port=5000)
```

### `Dockerfile`

Le Dockerfile est configuré pour créer une image Docker basée sur Python, installer les dépendances et exécuter l'application Flask. Voici son contenu :

```Dockerfile
# Utilisez une image de base Python
FROM python:3.9-slim

# Définissez le répertoire de travail
WORKDIR /app

# Copiez le fichier requirements.txt dans le conteneur
COPY requirements.txt .

# Installez les dépendances Python
RUN pip install --no-cache-dir -r requirements.txt

# Copiez le code de l'application Flask dans le conteneur
COPY app.py .

# Exposez le port 5000 pour que Flask puisse s'exécuter
EXPOSE 8000

# Commande pour lancer l'application
CMD ["python", "app.py"]
```

### `.dockerignore`

Le fichier .dockerignore spécifie les fichiers et dossiers à exclure lors de la construction de l'image Docker. Cela permet d’optimiser la taille de l’image.

## Instructions de déploiement

### 1. Construire l'image Docker

Exécutez la commande suivante depuis le dossier du projet pour créer l'image Docker :

```bash
docker build -t app_flask .
```

### 2. Démarrer le conteneur Docker

Pour lancer le conteneur, exécutez cette commande en mappant le port 5000 du conteneur au port 8000 de votre machine :

```bash
docker run -p 8000:5000 app_flask
```

Une fois le conteneur en cours d'exécution, vous verrez un message de confirmation, et vous pourrez accéder à l'application via http://localhost:8000.

### 3. Tester l'API

Dans un navigateur ou via un outil comme curl, vous pouvez accéder à l'API :

```plaintext
http://localhost:8000
```

Vous recevrez une réponse JSON similaire à :

```json
{"Bienvenue dans notre API!"}
```

## Arrêter le conteneur

Pour arrêter le conteneur, retournez au terminal et appuyez sur CTRL + C.

## Dépannage

### Erreurs fréquentes

- **Port déjà utilisé** : Si le port 8000 est déjà occupé, modifiez la commande docker run pour utiliser un autre port.

- **Docker Daemon non démarré** : Assurez-vous que Docker est bien lancé.
  
## Ressources supplémentaires

- [Documentation Flask](https://flask.palletsprojects.com/)
- [Documentation Docker](https://docs.docker.com/)

---

Ce guide devrait vous permettre de prendre en main l'application, de l'exécuter et de la tester en toute simplicité. N'hésitez pas à ajuster la configuration pour répondre à vos besoins spécifiques.
