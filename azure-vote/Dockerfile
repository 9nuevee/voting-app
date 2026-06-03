FROM python:3.13-slim

WORKDIR /app

# Copier le fichier des dépendances
COPY requirements.txt .

# Installer les dépendances
RUN pip install --no-cache-dir -r requirements.txt && \
  apt-get update && apt-get install -y curl && \
  rm -rf /var/lib/apt/lists/*

# Copier le reste du code de l'application
COPY . .

# Exposer le port 80
EXPOSE 80

# Définir les variables d'environnement par défaut
ENV REDIS=redis
ENV REDIS_PWD=""

# Healthcheck pour vérifier que l'application répond sur le port 80
HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl -f http://localhost:80/ || exit 1

# Lancer l'application via flask en forçant l'écoute sur le port 80
CMD ["python", "-m", "flask", "--app", "main", "run", "--host=0.0.0.0", "--port=80"]
