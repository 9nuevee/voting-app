# Leçon 2 : Conteneurisation de la Voting App avec Docker

## Objectif de cette étape
L'objectif est d'isoler notre application et sa base de données (Redis) dans des **conteneurs Docker**. Cela permet d'exécuter l'application facilement sur n'importe quelle machine sans se soucier des dépendances locales (comme l'absence de Python que nous avons vue précédemment !).

## 1. Le Dockerfile
Un `Dockerfile` est comme une recette de cuisine. Il explique à Docker comment fabriquer l'image de notre application. Voici nos choix techniques :

- **`FROM python:3.13-slim`** : Nous partons d'une image de base officielle Python 3.13 (la version recommandée dans le README). Le tag `-slim` signifie qu'elle est allégée (moins lourde à télécharger et plus sécurisée car moins d'outils inutiles).
- **`WORKDIR /app`** : On se place dans le dossier `/app` à l'intérieur du conteneur.
- **`COPY requirements.txt .` et `RUN pip install ...`** : On installe `flask` et `redis`. Nous utilisons `--no-cache-dir` pour ne pas garder les fichiers temporaires de téléchargement, réduisant ainsi la taille de l'image.
- **`HEALTHCHECK`** : C'est une sécurité. Docker va faire un appel `curl` régulier sur `http://localhost:80/` (le port de l'app). Si l'application ne répond plus, Docker le saura et affichera l'état `unhealthy`.
- **`CMD ["python", "-m", "flask", "--app", "main", "run", "--host=0.0.0.0", "--port=80"]`** : C'est la commande lancée au démarrage. Par défaut, Flask écoute sur `127.0.0.1:5000` (inaccessible depuis l'extérieur du conteneur). On force donc l'écoute sur toutes les adresses (`0.0.0.0`) et sur le **port 80** comme demandé.

## 2. Le fichier docker-compose.yml
Le `docker-compose.yml` permet d'orchestrer plusieurs conteneurs (notre application web + la base de données Redis).

- **Service `redis`** : Nous utilisons l'image `redis:alpine` (version allégée de Redis). Nous passons la commande `--requirepass` pour sécuriser la base de données avec un mot de passe.
- **Service `voting-app`** :
    - **`build: ./azure-vote`** : Indique à Compose de construire l'image avec notre `Dockerfile` avant de lancer le conteneur.
    - **`ports: "8080:80"`** : C'est le port mapping. Le port `8080` de notre machine Windows pointe vers le port `80` du conteneur (où tourne Flask).
    - **`environment`** : Nous fournissons les variables `REDIS=redis` (le nom du service Compose fait office de nom d'hôte grâce au réseau interne Docker) et `REDIS_PWD` (mot de passe identique à celui configuré dans le conteneur Redis).
    - **`depends_on: [redis]`** : S'assure que Redis démarre *avant* notre application web.

## Comment ça fonctionne ensemble ?
Docker crée un **réseau privé** entre les deux conteneurs. 
L'application Python va demander à se connecter à `redis` (via la variable d'environnement). Docker va résoudre ce nom et diriger la requête vers le conteneur Redis, de manière totalement invisible et sécurisée.
