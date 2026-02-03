# 🚀 Deployment Repository (CD)

Repository de déploiement pour la chaîne DevOps complète.

## Architecture

```
Backend CI (GitHub) → Docker Hub
       ↓
Deployment CD (ce repo) → Docker Compose → Application Live
```

## Utilisation

### 1. Initialiser Git
```bash
cd deployment-ci
git init
git remote add origin https://github.com/elhadjsow/deployment-ci.git
git add .
git commit -m "Initial commit: Deployment configuration"
git push -u origin main
```

### 2. Configurer Jenkins
1. Créer un nouveau job Pipeline : `deployment-pipeline`
2. Pointer vers ce repo GitHub
3. Script path : `Jenkinsfile`

### 3. Déclencher le déploiement
```bash
git push  # Jenkins déclenche automatiquement
```

## Fonctionnement

- **Stage 1**: Checkout du code de déploiement
- **Stage 2**: Télécharge les dernières images depuis Docker Hub
- **Stage 3**: Arrête le stack actuel
- **Stage 4**: Lance le nouveau stack avec `docker-compose up -d`
- **Stage 5**: Vérifie que tout fonctionne

## Services déployés

- **Backend API**: http://localhost:8000
- **PostgreSQL**: localhost:5432

## Notes

- Le docker-compose.yml pointe vers l'image `elhadjsow/backend-certificat:latest`
- Les images sont automatiquement mises à jour depuis Docker Hub
- Aucune compilation locale requise

---

**Workflow complet:**
1. Développeur push code → Backend CI build/test/push Docker
2. Administrateur push déploiement → Deployment CD deploy sur prod
