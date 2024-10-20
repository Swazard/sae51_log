### Collecte des Logs avec Docker : Grafana, Loki, Alloy et Nginx

Dans cette documentation, nous allons expliquer comment collecter des logs avec une solution basée sur Grafana pour la visualisation, Loki pour la centralisation et le stockage des logs, et Alloy pour la collecte des logs générés par Nginx. Cette approche permet de monitorer les logs des services de manière centralisée via des tableaux de bord Grafana.

---

## **1. Explication des Composants**

- **Grafana** : Outil de visualisation open-source qui permet de créer des tableaux de bord (dashboards) interactifs. Dans cette configuration, Grafana est utilisé pour afficher et analyser les logs collectés par Loki.
  
- **Loki** : Un agrégateur de logs conçu pour fonctionner de manière similaire à Prometheus (mais pour les logs). Loki indexe les logs par les labels et stocke les fichiers logs bruts sans les analyser, ce qui le rend plus léger.

- **Alloy** : C’est l’agent qui collecte les logs sur les serveurs (ou conteneurs). Il récupère les fichiers de logs de Nginx (et d'autres services) et les envoie vers Loki pour stockage et indexation, ici il utilise un volume partagé entre le docker nginx, la machine et le docker Alloy.

- **Nginx** : Serveur HTTP qui dans cet exemple génère des logs (les accès au serveur, les erreurs, etc.). Ces logs seront envoyés vers Loki via Alloy.

---

## **2. Installation de Docker et Docker Compose**

Avant de démarrer, il faut s'assurer que Docker et Docker Compose sont installés sur votre machine.

### **2.1 Installation de Docker**
Pour installer Docker, suivez les instructions spécifiques à votre OS :

**Linux (Ubuntu) :**
```bash
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```


### **2.2 Installation de Docker Compose**
**Il se peut que le packet ne soit pas installer sur la machine Linux :**
```bash
sudo apt update
sudo apt install docker-compose
```

---

## **3. Docker Compose pour la Collecte de Logs**

Le fichier `docker-compose.yml` permet de déployer Grafana, Loki, Alloy et Nginx dans des conteneurs Docker.


### **3.2 Explication des Services**

- **Grafana** : Il expose le port 3000 pour l'accès au tableau de bord. Vous pouvez y accéder via `http://localhost:3000`.
- **Loki** : Utilisé pour stocker les logs. Le port 3100 est exposé pour la communication avec Alloy et Grafana.
- **Alloy** : Cet agent lit les fichiers de logs générés par Nginx (dans `/var/log/nginx`) et les envoie à Loki.
- **Nginx** : Il génère des logs d'accès et d'erreurs qui sont collectés par Alloy et centralisés dans Loki.

---

## **4. Instructions pour Lancer le Projet**

### **4.1 Étape 1 : Cloner le projet**
Téléchargez le fichier `docker-compose.yml` et les fichiers de configuration nécessaires.

```bash
git clone https://github.com/Swazard/sae51_log.git
cd sae51_log
```
ou en ssh
```
git clone git@github.com:Swazard/sae51_log.git
cd sae51_log
```
### **4.2 Étape 2 : Lancer Docker Compose**
Exécutez la commande suivante pour démarrer tous les services :

```bash
docker-compose up -d
```

Cela va :
- Démarrer un serveur Nginx pour générer des logs.
- Démarrer Loki et Alloy pour collecter et centraliser ces logs.
- Démarrer Grafana pour afficher les logs dans un tableau de bord.

### **4.3 Étape 3 : Accéder à Grafana**
Accédez à Grafana en ouvrant votre navigateur à l’adresse suivante :

```
http://localhost:3000
```

Vous pouvez ensuite configurer Loki comme source de données dans Grafana et créer un tableau de bord pour visualiser les logs collectés.

---

- [Grafana Documentation](https://grafana.com/docs/)
- [Loki Documentation](https://grafana.com/docs/loki/latest/)


Kylian Laloux
Yann Beaudouin
