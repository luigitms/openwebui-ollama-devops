# OpenWebUI Ollama DevOps Platform

Projet DevOps local permettant de déployer une plateforme de chatbot IA open source basée sur **Open WebUI** et **Ollama**.

L’objectif de cette V1 est de proposer un déploiement **100 % local**, sans dépendance cloud, avec deux modes de lancement :

* Docker Compose pour un démarrage rapide ;
* Kubernetes local avec k3d et Helm pour simuler un environnement cloud-native.

Ce projet ne vise pas à revendiquer la création d’Open WebUI. Open WebUI est utilisé comme application open source de référence. La valeur principale du repository repose sur la partie DevOps : conteneurisation, orchestration Kubernetes, packaging Helm, automatisation et documentation.

---

## Objectifs du projet

* Déployer Open WebUI avec Ollama en local
* Fournir un environnement reproductible avec Docker Compose
* Déployer l’application sur Kubernetes local avec k3d
* Packager l’application avec Helm
* Télécharger automatiquement un modèle Ollama léger
* Préparer une base CI/CD avec GitHub Actions
* Documenter les commandes et les étapes de déploiement
* Préparer une future V2 avec déploiement cloud AWS

---

## Stack technique

* Open WebUI
* Ollama
* Docker
* Docker Compose
* Kubernetes local
* k3d
* Helm
* GitHub Actions

---

## Architecture locale

```text
Utilisateur
   |
   | http://localhost:3000 ou http://localhost:3001
   v
Open WebUI
   |
   | OLLAMA_BASE_URL
   v
Ollama
   |
   v
Modèle local : llama3.2:1b
```

---

## Modes de déploiement

### 1. Déploiement local avec Docker Compose

Ce mode permet de lancer rapidement Open WebUI et Ollama en local.

Commande :

```bash
docker compose up -d
```

Vérification :

```bash
docker ps
```

Accès à l’application :

```text
http://localhost:3000
```

Le modèle Ollama utilisé pour la démonstration est :

```text
llama3.2:1b
```

---

### 2. Déploiement Kubernetes local avec k3d et Helm

Ce mode permet de déployer Open WebUI et Ollama dans un cluster Kubernetes local.

Création du cluster k3d :

```bash
k3d cluster create openwebui-local --agents 1
```

Vérification du cluster :

```bash
kubectl get nodes
```

Validation du chart Helm :

```bash
helm lint ./kubernetes/helm/openwebui-ollama
```

Déploiement avec Helm :

```bash
helm upgrade --install openwebui ./kubernetes/helm/openwebui-ollama -n openwebui --create-namespace -f ./kubernetes/helm/openwebui-ollama/values-local.yaml --timeout 20m
```

Vérification du déploiement :

```bash
helm list -n openwebui
kubectl get pods -n openwebui
kubectl get svc -n openwebui
kubectl get pvc -n openwebui
```

Accès à Open WebUI via Kubernetes :

```bash
kubectl port-forward -n openwebui svc/openwebui-ollama-openwebui 3001:8080
```

URL locale :

```text
http://localhost:3001
```

Vérification du modèle Ollama dans Kubernetes :

```bash
kubectl exec -n openwebui deploy/openwebui-ollama-ollama -- ollama list
```

---

## Structure du repository

```text
.
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── k3d-deploy-test.yml
├── kubernetes/
│   └── helm/
│       └── openwebui-ollama/
│           ├── Chart.yaml
│           ├── values.yaml
│           ├── values-local.yaml
│           └── templates/
├── screenshots/
├── scripts/
│   └── local/
├── .env.example
├── .gitignore
├── docker-compose.yml
└── README.md
```

---

## Captures d’écran

Les captures d’écran du projet sont disponibles dans le dossier :

```text
screenshots/
```

Captures principales :

```text
01-docker-compose-containers-running.png
02-openwebui-docker-localhost-3000.png
03-helm-lint-success.png
04-k3d-cluster-nodes-ready.png
05-helm-release-deployed.png
06-helm-status-openwebui.png
07-kubernetes-pods-running.png
08-kubernetes-services.png
09-kubernetes-persistent-volumes.png
10-kubernetes-port-forward-localhost-3001.png
11-openwebui-kubernetes-localhost-3001.png
12-ollama-model-available-in-kubernetes.png
13-github-actions-ci-success.png
14-github-actions-k3d-deploy-test-success.png
```

---

## CI/CD GitHub Actions

Une première base GitHub Actions est prévue pour valider automatiquement le projet.

Objectifs de la CI :

* vérifier la syntaxe Docker Compose ;
* vérifier la structure du repository ;
* valider le chart Helm ;
* préparer un futur test de déploiement k3d automatisé.

La partie GitHub Actions sera finalisée après validation complète du déploiement local.

---

## Nettoyage local

Arrêter Docker Compose :

```bash
docker compose down
```

Supprimer le déploiement Helm :

```bash
helm uninstall openwebui -n openwebui
```

Supprimer le cluster k3d :

```bash
k3d cluster delete openwebui-local
```

---

## Roadmap

* [x] Créer la structure du projet
* [x] Ajouter le déploiement Docker Compose
* [x] Ajouter le chart Helm
* [x] Ajouter le déploiement Kubernetes local
* [x] Ajouter les captures d’écran
* [x] Ajouter GitHub Actions
* [ ] Rédiger le README final complet
* [ ] Préparer une V2 cloud AWS

---

## Roadmap V2 AWS

Une future version du projet pourra intégrer :

* Terraform
* AWS EKS
* AWS ECR
* Load Balancer AWS
* Ingress Controller
* HTTPS
* Monitoring Prometheus / Grafana
* Pipeline CI/CD complet de déploiement cloud

---

## Auteur

Projet réalisé par Luigi THOMAS dans le cadre d’un portfolio DevOps orienté infrastructure, automatisation et déploiement d’applications IA self-hosted.