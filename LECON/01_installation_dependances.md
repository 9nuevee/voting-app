# Leçon 1 : Installation des dépendances et Environnements Virtuels

## Objectif de cette étape
L'objectif est de préparer notre environnement de travail local pour que l'application Azure Voting App puisse s'exécuter. Comme indiqué dans le `README.md`, cette application nécessite Python et certaines bibliothèques spécifiques (`flask` et `redis`).

## 1. Pourquoi utiliser un environnement virtuel (`virtualenv`) ?
Dans le monde Python, il est **fortement recommandé** d'utiliser des environnements virtuels pour chaque projet. 

**Explication technique :**
Un environnement virtuel est un dossier isolé (souvent appelé `venv` ou `.venv`) qui contient sa propre copie de l'interpréteur Python et de son gestionnaire de paquets (`pip`). 
- **L'intérêt** : Cela permet d'isoler les dépendances de ce projet du reste de votre ordinateur. Si demain vous avez un autre projet qui nécessite une ancienne version de `flask`, les deux projets n'entreront pas en conflit car chacun aura son propre environnement.
- **Fonctionnement** : On crée l'environnement avec la commande `python -m venv venv`, puis on l'active (avec `.\venv\Scripts\activate` sur Windows). Une fois activé, toutes les installations de bibliothèques se font uniquement dans ce dossier isolé.

## 2. Les dépendances du projet
Le projet utilise le gestionnaire de paquets de Python appelé **Pip** (Pip Installs Packages) pour télécharger et installer des bibliothèques externes depuis internet.

Voici les deux dépendances dont nous avons besoin :
- **Flask** : C'est un micro-framework web pour Python. Il permet de créer des applications web (serveurs, routes, API) de manière très simple et légère. C'est lui qui va faire tourner le serveur web de notre application de vote.
- **Redis** : C'est une bibliothèque Python qui permet à notre application de communiquer avec une base de données de type Cache Redis. Dans notre cas, elle servira à stocker les résultats des votes.

## 3. Ce qui a été tenté
Pour installer les dépendances, la procédure standard (et celle que j'ai tenté d'exécuter pour vous) est la suivante :

```powershell
# 1. Création de l'environnement virtuel
python -m venv venv

# 2. Activation de l'environnement
.\venv\Scripts\activate

# 3. Installation des dépendances avec Pip
pip install flask redis
```

> [!WARNING]
> **Problème rencontré :** Lors de l'exécution, le système a indiqué que **Python n'est pas installé** (ou n'est pas accessible dans vos variables d'environnement Windows). L'alias `python` redirige actuellement vers le Microsoft Store.

## Prochaine étape
Avant de pouvoir continuer, il est nécessaire d'installer Python (version 3.13 recommandée comme indiqué dans le README) sur votre machine Windows.

**Que souhaitez-vous faire ?**
1. Vous installez Python de votre côté, puis nous relançons l'installation.
2. Nous passons directement sur une approche par conteneur (Docker) si vous ne souhaitez pas installer Python localement.
