# TP14 : Kubernetes avec K3s

Ce dépôt contient le travail réalisé dans le cadre du TP14. L'objectif est de déployer un cluster Kubernetes léger avec **K3s** (via Vagrant et Ansible) et d'explorer les objets Kubernetes fondamentaux sur une application de démonstration (`traefik/whoami`).

## 🎯 Objectifs pédagogiques

À l'issue de ce TP, les compétences suivantes ont été mises en pratique :
- Déployer un cluster K3s multi-nœuds de manière reproductible (IaC).
- Utiliser `kubectl` pour l'administration quotidienne (get, describe, logs, apply, rollout...).
- Maîtriser les objets de base : Namespace, Deployment, Service, Ingress, ConfigMap, Secret, PVC et HPA.
- Configurer la santé des conteneurs (liveness/readiness probes) et les limites de ressources.
- Réaliser des mises à jour progressives (rolling update) et des rollbacks.
- Gérer le stockage persistant et l'autoscaling.

## 🛠️ Prérequis

- **VirtualBox** (avec la virtualisation imbriquée activée)
- **Vagrant**
- **Ansible**
- **kubectl** installé sur la machine hôte (VM de lab)
- **Ressources** : VM de lab Ubuntu avec au moins 4 vCPU et 8 Go de RAM.

## 📂 Structure du projet

- `k3s-lab/` : Contient le `Vagrantfile`, le `playbook.yml` et le rôle Ansible `k3s` pour provisionner le cluster.
- `k8s-starter/` : Contient les manifestes YAML (`01-namespace.yaml` à `07-hpa.yaml`) pour déployer l'application.
- `captures/` : Contient les captures d'écran des livrables.

## 🚀 Contenu du TP

### Partie 1 : Cluster K3s avec Vagrant + Ansible
- Provisionnement d'un serveur K3s et d'un agent via Vagrant.
- Automatisation de l'installation avec Ansible (rôle `k3s`).
- Récupération du fichier `kubeconfig` et vérification avec `kubectl get nodes`.

### Partie 2 : Pods et Deployments
- Création manuelle d'un Pod et observation de son comportement.
- Déploiement de l'application `whoami` via le `Deployment`.
- Observation de l'auto-réparation (recréation des Pods supprimés) et de la répartition sur les nœuds.

### Partie 3 : Services et Ingress
- Exposition interne (ClusterIP) et routage HTTP via Traefik (Ingress).
- Test de résolution DNS interne au cluster.
- Configuration de l'Ingress pour `whoami.lab.local`.

### Partie 4 : ConfigMap et Secret
- Injection de variables d'environnement via `ConfigMap`.
- Création et injection d'un `Secret` (clé d'API).
- Observation de la persistance des variables dans les Pods.

### Partie 5 : Santé, ressources et déploiements progressifs
- Ajout de `readinessProbe` et `livenessProbe` (simulation de panne via `/health`).
- Configuration des `requests` et `limits` (test de l'état `OOMKilled`).
- Test de `rolling update` (mise à jour de version) et de `rollback` suite à une image corrompue.

### Partie 6 : Stockage persistant
- Déploiement de PostgreSQL avec un `PersistentVolumeClaim` (PVC).
- Utilisation du provisioner `local-path` de K3s.
- Test de persistance des données après suppression d'un Pod.

### Partie 7 : HorizontalPodAutoscaler (HPA)
- Configuration de l'autoscaling basé sur l'utilisation CPU.
- Génération de charge pour observer la montée et la descente en charge du nombre de replicas.

## 📸 Livrables

Les captures d'écran justifiant la réalisation des étapes sont disponibles dans le dossier `captures/` :

- `cluster-nodes.png` : Nœuds du cluster `Ready` et Pods système.
- `rebuild.png` : Preuve de la recréation du cluster (`vagrant destroy -f && vagrant up`).
- `ingress.png` : Résultat du `curl http://whoami.lab.local` et de la route `/api`.
- `probes-restart.png` : Pod redémarré par la `livenessProbe`.
- `rollout-undo.png` : Historique et retour arrière après échec de déploiement.
- `pvc-persistence.png` : Preuve de la conservation des données PostgreSQL.
- `hpa.png` : Capture du HPA en action (`kubectl get hpa -w`).

## ⚠️ Sécurité et Bonnes Pratiques

Conformément aux consignes de sécurité et aux bonnes pratiques DevOps :
- **Ne jamais versionner** le dossier `.vagrant/` (contient les clés SSH privées des VMs).
- **Ne jamais versionner** le fichier `k3s.yaml` (contient les accès administrateur au cluster).
- **Ne jamais versionner** les fichiers `Secret` (les mots de passe doivent être créés à la volée ou gérés via un gestionnaire de secrets externe).

Un fichier `.gitignore` est présent à la racine pour prévenir tout ajout accidentel de ces fichiers sensibles.

## 🏁 Défi (Bonus)

- Ajout d'un troisième nœud au cluster modifiant uniquement le `Vagrantfile`.
- Test de la commande `kubectl drain` pour observer le déplacement des Pods.
- Mise en place d'un `PodDisruptionBudget` (PDB) pour empêcher l'éviction simultanée de tous les Pods.
