# ☸️ TP14 : Déploiement et Administration d'un Cluster Kubernetes avec K3s

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![K3s](https://img.shields.io/badge/K3s-FFC61C?style=for-the-badge&logo=k3s&logoColor=black)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![Vagrant](https://img.shields.io/badge/vagrant-%231563FF.svg?style=for-the-badge&logo=vagrant&logoColor=white)

## 📝 Présentation du projet
Ce dépôt contient l'intégralité du TP14. L'objectif était de déployer un cluster Kubernetes multi-nœuds léger avec **K3s** (via Vagrant et Ansible), puis d'explorer les objets Kubernetes fondamentaux (Deployments, Services, Ingress, ConfigMaps, Secrets, PVC, HPA) sur une application de démonstration.

## 📂 Structure du Dépôt
- 📁 `k3s-lab/` : Infrastructure as Code (Vagrantfile, playbook et rôles Ansible).
- 📁 `k8s-starter/` : Manifestes YAML de l'application.
- 📁 `captures_ecran/` : Preuves visuelles de validation du TP.
- 📄 `rapport_tp14.pdf` : Rapport détaillé avec les réponses théoriques.

---

## 🚀 Validation des étapes (Livrables)

<details>
<summary><b>Partie 1 : Cluster K3s avec Vagrant + Ansible (Cliquez pour voir)</b></summary>
<br>

**Vérification du cluster (2 nœuds Ready) et des pods système :**
![Cluster Nodes](captures_ecran/00_cluster-nodes.png)

**Recréation du cluster complet (destroy & up) :**
![Rebuild 1](captures_ecran/01_rebuild.png)
![Rebuild 2](captures_ecran/01_rebuild2.png)
</details>

<details>
<summary><b>Partie 2 : Pods et Deployments (Cliquez pour voir)</b></summary>
<br>

**Création manuelle d'un Pod et suppression :**
![Pod Manual](captures_ecran/02_pod-manual.png)
![Pod Delete](captures_ecran/04a_pod_delete.png)

**Application du Deployment et auto-réparation (recréation du pod) :**
![Deployment Apply](captures_ecran/03_deployment_apply.png)
![Pod Recreation](captures_ecran/04_pod_recreation.png)

**Exploration (describe, logs, exec) :**
![Pod Describe](captures_ecran/05a_pod_describe.png)
![Pod Explore](captures_ecran/05b_pod_explore.png)
</details>

<details>
<summary><b>Partie 3 : Services et Ingress (Cliquez pour voir)</b></summary>
<br>

**Vérification du Service ClusterIP et des Endpoints :**
![Service Endpoints](captures_ecran/06_service_endpoints.png)

**Test de résolution DNS interne au cluster :**
![DNS Internal](captures_ecran/08_dns_internal.png)
</details>

<details>
<summary><b>Partie 4 : ConfigMap et Secret (Cliquez pour voir)</b></summary>
<br>

**Injection des variables d'environnement (ConfigMap/Secret) dans l'application :**
![ConfigMap Secret](captures_ecran/09_configmap-secret.png)
</details>

<details>
<summary><b>Partie 5 : Santé, ressources et déploiements progressifs (Cliquez pour voir)</b></summary>
<br>

**Test de la LivenessProbe (simulation de panne 500 et redémarrage) :**
![Probes Restart](captures_ecran/10_probes-restart.png)

**Rolling Update, échec (ImagePullBackOff) et Rollback :**
![Rollout Undo](captures_ecran/11_rollout-undo.png)

**Test de dépassement de mémoire (OOMKilled) et restauration :**
![OOM Killed](captures_ecran/12_OOMKilled.png)
![Deployment Restored](captures_ecran/13_deployment-restored.png)
</details>

<details>
<summary><b>Partie 6 : Stockage persistant (Cliquez pour voir)</b></summary>
<br>

**Preuve de persistance des données PostgreSQL après suppression du Pod :**
![PVC Persistence](captures_ecran/14_pvc-persistence.png)
</details>

<details>
<summary><b>Partie 7 : HorizontalPodAutoscaler (Cliquez pour voir)</b></summary>
<br>

**Mise à l'échelle automatique (HPA) : Montée du CPU et augmentation des replicas :**
![HPA](captures_ecran/15_hpa.png)
</details>

---

## 🔒 Sécurité et Bonnes Pratiques
Conformément aux règles de sécurité DevOps :
- Le dossier `.vagrant/` (contenant les clés SSH privées) est exclu via le `.gitignore`.
- Le fichier `k3s.yaml` (kubeconfig) n'est pas versionné.
- Les Secrets ne sont pas écrits en dur dans les fichiers YAML.
