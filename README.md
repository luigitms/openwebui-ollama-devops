# OpenWebUI Ollama DevOps Platform

Projet DevOps local permettant de déployer Open WebUI avec Ollama via Docker Compose, puis Kubernetes local avec Helm.

## Objectif

L’objectif de cette V1 est de proposer un déploiement 100 % local d’une plateforme de chatbot IA open source, sans dépendance cloud.

Cette première version met en avant :

- un déploiement local avec Docker Compose ;
- un futur déploiement Kubernetes local avec k3d ;
- un packaging applicatif avec Helm ;
- une intégration CI/CD avec GitHub Actions ;
- une documentation claire et reproductible.

## Stack technique

- Open WebUI
- Ollama
- Docker
- Docker Compose
- Kubernetes local avec k3d
- Helm
- GitHub Actions

## Roadmap

- [x] Créer la structure du projet
- [ ] Ajouter le déploiement Docker Compose
- [ ] Ajouter le chart Helm
- [ ] Ajouter le déploiement Kubernetes local
- [ ] Ajouter GitHub Actions
- [ ] Ajouter les captures d’écran
- [ ] Rédiger le README final
- [ ] Préparer une V2 cloud AWS