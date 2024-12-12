# Mauvaises Pratiques d'Exploitation des Clusters Kubernetes

## 1. Mauvaises Pratiques en Matière de Sécurité
- **Exposer l'API Kubernetes au public sans restriction**  
  Laisser l'API Kubernetes accessible à tout le monde sans contrôle d'accès ou protection adéquate (pare-feu, VPN).
- **Utiliser le compte `kubeadmin` pour les opérations quotidiennes**  
  Ce compte a des privilèges étendus et ne devrait être utilisé que pour des tâches critiques.
- **Ne pas limiter les permissions RBAC (Role-Based Access Control)**  
  Attribuer des permissions larges (comme `cluster-admin`) aux utilisateurs ou applications sans réflexion.
- **Ne pas configurer de politiques réseau (`NetworkPolicies`)**  
  Permettre la communication non restreinte entre tous les Pods favorise les risques de mouvements latéraux en cas de compromission.
- **Utiliser des images de conteneurs non vérifiées**  
  Tirer des images depuis des registres publics sans vérifier leur provenance ou contenu.
- **Ne pas activer l’authentification mutuelle TLS entre composants du cluster**  
  Laisser des communications internes non sécurisées expose à des attaques "man-in-the-middle".
- **Oublier de scanner les vulnérabilités des images**  
  Ne pas utiliser des outils comme Trivy ou Clair pour détecter les vulnérabilités dans les images des conteneurs.

---

## 2. Mauvaises Pratiques en Gestion des Ressources
- **Ne pas configurer les limites de ressources (`resource limits`)**  
  Laisser des Pods sans limites de CPU et mémoire peut entraîner des goulets d'étranglement ou des crashs.
- **Ne pas utiliser des `requests` pour les ressources**  
  Empêche Kubernetes de planifier efficacement les Pods.
- **Sous-estimer ou sur-allouer les ressources**  
  Des `requests` ou `limits` mal configurés peuvent entraîner des pertes de performance ou un gaspillage de ressources.
- **Ne pas surveiller les métriques des ressources**  
  Ignorer les outils comme Prometheus, Grafana ou Kubernetes Metrics Server.

---

## 3. Mauvaises Pratiques en Gestion des Pods et Déploiements
- **Ne pas utiliser de contrôleurs (`Deployments`, `StatefulSets`, etc.)**  
  Lancer directement des Pods rend leur gestion manuelle et limite la résilience.
- **Ne pas utiliser de `liveness` et `readiness probes`**  
  Ne pas configurer ces sondes empêche Kubernetes de détecter et gérer les conteneurs défaillants.
- **Utiliser `latest` comme tag pour les images de conteneurs**  
  Cela rend les déploiements imprévisibles car le contenu de l'image peut changer sans préavis.
- **Ne pas utiliser des stratégies de déploiement (`RollingUpdate`, `Recreate`)**  
  Ignorer ces stratégies peut entraîner des temps d'arrêt imprévus.
- **Négliger les annotations et labels**  
  Ne pas ajouter de métadonnées standardisées rend la gestion et le filtrage plus difficiles.

---

## 4. Mauvaises Pratiques en Gestion des Volumes
- **Ne pas configurer de stratégies de sauvegarde pour les volumes**  
  Ne pas sauvegarder les données critiques stockées sur les volumes persistants.
- **Utiliser des PVC sans gérer leur cycle de vie**  
  Laisser des PVC orphelins ou inutilisés gaspille de l'espace de stockage.
- **Ignorer l’utilisation des quotas de stockage**  
  Ne pas appliquer de quotas peut entraîner un épuisement des ressources partagées.

---

## 5. Mauvaises Pratiques en Observabilité
- **Ne pas activer de journaux centralisés**  
  Laisser les logs uniquement au niveau des Pods rend leur collecte et analyse complexes.
- **Ne pas configurer d’alertes sur les métriques critiques**  
  Absence d’alertes pour les anomalies de performance, les échecs ou les incidents de sécurité.
- **Ignorer les traces des applications distribuées**  
  Ne pas utiliser d'outils comme Jaeger ou Zipkin pour suivre les requêtes dans les microservices.

---

## 6. Mauvaises Pratiques en Scalabilité et Haute Disponibilité
- **Ne pas activer l’auto-scaling (`HorizontalPodAutoscaler`, `ClusterAutoscaler`)**  
  Négliger les mécanismes d'auto-scaling peut entraîner des interruptions sous forte charge.
- **Ne pas répartir les workloads sur plusieurs zones de disponibilité**  
  Cela augmente la vulnérabilité en cas de panne d’une zone.
- **Exécuter tous les Pods sur le même nœud**  
  Ne pas configurer de règles d’affinité ou de tolérance pour éviter la concentration des Pods critiques.

---

## 7. Mauvaises Pratiques en Maintenance
- **Ne pas appliquer de mises à jour régulières**  
  Utiliser des versions obsolètes de Kubernetes ou des images d'applications non patchées.
- **Ne pas tester les modifications dans un environnement de staging**  
  Appliquer des changements directement en production sans validation préalable.
- **Ignorer les sauvegardes de l’état du cluster (etcd)**  
  Ne pas sauvegarder les données critiques comme les configurations et secrets stockés dans etcd.
- **Ne pas nettoyer les ressources inutilisées**  
  Laisser des ConfigMaps, Secrets ou Services orphelins encombrer le cluster.

---

## 8. Mauvaises Pratiques en Réseau
- **Ne pas utiliser un DNS interne robuste**  
  Négliger la configuration de CoreDNS ou des mécanismes de résolution de noms peut entraîner des erreurs.
- **Ne pas limiter l'accès aux services exposés**  
  Ouvrir des Services avec `type: LoadBalancer` ou `NodePort` sans règles de sécurité appropriées.
- **Négliger l’encryptage des communications**  
  Ne pas activer TLS entre les services internes du cluster.

---

## Résumé
Adopter de bonnes pratiques d'exploitation Kubernetes nécessite une attention particulière aux aspects de **sécurité**, **gestion des ressources**, **observabilité**, et **résilience**. En évitant ces mauvaises pratiques, vous réduirez les risques opérationnels et garantirez une gestion efficace et sécurisée de vos clusters.
