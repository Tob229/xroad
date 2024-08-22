

# 1. Introduction à X-Road

## 1.1 Présentation de X-Road

### 1.1.1 Qu'est-ce que X-Road ?
X-Road est une plateforme de gestion de données interopérable et sécurisée conçue pour l'échange d'informations entre les systèmes d'information. Elle est largement utilisée pour permettre la communication entre différents services publics et organisations, garantissant la sécurité, la confidentialité et l'intégrité des données échangées.

### 1.1.2 Architecture de X-Road
X-Road repose sur une architecture distribuée comprenant plusieurs composants principaux :
- **Serveur Central** : Le serveur central joue un rôle crucial dans la gestion des configurations globales, des certificats, et dans l'enregistrement des serveurs de sécurité. Il assure la fédération et la synchronisation des informations entre différents membres de l'infrastructure X-Road.
- **Serveurs de Sécurité** : Ces serveurs agissent comme des intermédiaires sécurisés entre les systèmes clients et les systèmes fournisseurs de services. Ils gèrent les requêtes entrantes, appliquent des règles de sécurité, et assurent la traçabilité des transactions.
- **Clients** : Les clients sont des sous-systèmes des membres X-Road qui consomment ou fournissent des services via la plateforme.
- **Certification Authority (CA)** : Les autorités de certification sont responsables de la délivrance et de la gestion des certificats numériques pour X-Road, garantissant l'authenticité et l'intégrité des communications.

### 1.1.3 Fonctionnalités Clés
- **Interopérabilité** : Permet la connexion de systèmes hétérogènes au sein d'une infrastructure partagée.
- **Sécurité** : Chiffrement des données en transit, authentification des entités, et gestion des autorisations.
- **Fiabilité** : Suivi des transactions, journalisation, et capacité de résilience en cas de pannes.

### 1.1.4 Cas d'Utilisation
- **E-gouvernement** : Intégration des services publics pour permettre des échanges sécurisés entre différentes agences gouvernementales.
- **Santé** : Partage sécurisé des dossiers médicaux entre hôpitaux, cliniques, et autres institutions de santé.
- **Secteur privé** : Échange sécurisé de données entre entreprises et institutions financières, par exemple.

## 1.2 Installation et Déploiement

### 1.2.1 Prérequis
Avant de commencer l'installation de X-Road, assurez-vous d'avoir les prérequis suivants :
- **Environnement Docker** : X-Road est souvent déployé en utilisant Docker, un outil permettant de créer, déployer, et exécuter des applications dans des conteneurs.
- **Docker Compose** : Utilisé pour définir et gérer des applications multi-conteneurs. Il simplifie la gestion des services X-Road, en permettant de déployer le serveur central et les serveurs de sécurité avec un seul fichier de configuration.
- **Certificats SSL** : Pour sécuriser les communications, des certificats SSL sont nécessaires pour le serveur central et les serveurs de sécurité.

### 1.2.2 Déploiement avec Docker et Docker Compose
1. **Préparer le Fichier Docker Compose**
   - Créez un fichier `docker-compose.yml` pour définir les services nécessaires (serveur central, serveurs de sécurité).
   - Exemple basique de configuration :
     ```yaml
        version: '3.7'
        services:
        cs:
            image: niis/xroad-central-server:jammy-7.4.2
            container_name: cs
            environment:
            XROAD_TOKEN_PIN: 123456xrd!
            ports:
            - 4000:4000
            - 8888:8888
            volumes:
            - cs-db-volume:/var/lib/postgresql/14/main
            - cs-conf-volume:/etc/xroad
            - cs-test-ca-volume:/home/ca
            networks:
            - xroad-network

        ss1:
            image: niis/xroad-security-server-sidecar:7.4.2
            container_name: ss1
            environment:
            XROAD_TOKEN_PIN: 123456xrd!
            XROAD_ADMIN_USER: xrd
            XROAD_ADMIN_PASSWORD: secret
            XROAD_LOG_LEVEL: INFO
            ports:
            - 1000:4000
            - 1080:8080
            - 1443:8443
            volumes:
            - ss1-db-volume:/var/lib/postgresql/12/main
            - ss1-conf-volume:/etc/xroad
            - ss1-archive-volume:/var/lib/xroad
            networks:
            - xroad-network
            depends_on:
            - cs

        ss2:
            image: niis/xroad-security-server-sidecar:7.4.2
            container_name: ss2
            environment:
            XROAD_TOKEN_PIN: 123456xrd!
            XROAD_ADMIN_USER: xrd
            XROAD_ADMIN_PASSWORD: secret
            XROAD_LOG_LEVEL: INFO
            ports:
            - 2000:4000
            - 2080:8080
            - 2443:8443
            volumes:
            - ss2-db-volume:/var/lib/postgresql/12/main
            - ss2-conf-volume:/etc/xroad
            - ss2-archive-volume:/var/lib/xroad
            networks:
            - xroad-network
            depends_on:
            - cs

        ss3:
            image: niis/xroad-security-server-sidecar:7.4.2
            container_name: ss3
            environment:
            XROAD_TOKEN_PIN: 123456xrd!
            XROAD_ADMIN_USER: xrd
            XROAD_ADMIN_PASSWORD: secret
            XROAD_LOG_LEVEL: INFO
            ports:
            - 3000:4000
            - 3080:8080
            - 3443:8443
            volumes:
            - ss3-db-volume:/var/lib/postgresql/12/main
            - ss3-conf-volume:/etc/xroad
            - ss3-archive-volume:/var/lib/xroad
            networks:
            - xroad-network
            depends_on:
            - cs

        volumes:
        cs-db-volume:
            name: cs-db-data
        cs-conf-volume:
            name: cs-conf-data
        cs-test-ca-volume:
            name: cs-test-ca-data
        ss1-db-volume:
            name: ss1-db-data
        ss1-conf-volume:
            name: ss1-conf-data
        ss1-archive-volume:
            name: ss1-archive-data
        ss2-db-volume:
            name: ss2-db-data
        ss2-conf-volume:
            name: ss2-conf-data
        ss2-archive-volume:
            name: ss2-archive-data
        ss3-db-volume:
            name: ss3-db-data
        ss3-conf-volume:
            name: ss3-conf-data
        ss3-archive-volume:
            name: ss3-archive-data

        networks:
        xroad-network:
            driver: bridge

     ```

2. **Structure de l'environnement**
L'environnement de test local se compose de quatre conteneurs Docker :

- **cs (Central Server)** : Serveur central qui inclut également un CA de test avec OCSP et TSA.
- **ss1 (Management Security Server)** : Serveur de sécurité de gestion.
- **ss2 (Security Server)** : Serveur de sécurité utilisé comme consommateur de services.
- **ss3 (Security Server)** : Serveur de sécurité utilisé comme fournisseur de services.

Chaque conteneur a trois volumes où la configuration du conteneur est persistée, permettant ainsi de détruire et recréer les conteneurs sans perdre la configuration de l'environnement.


3. **Démarrage de l'environnement**

Pour démarrer l'environnement, exécutez la commande suivante :

```bash
docker-compose up -d
```

4. **Vérification des journaux**

Vous pouvez vérifier les journaux des conteneurs pour vous assurer que tout fonctionne correctement :

```bash
docker-compose logs -f
```

5. **Configuration de l'environnement**

Après avoir démarré les conteneurs, vous devez configurer l'environnement en suivant ces étapes :

- **Adresse du Serveur Central (cs)** : Utilisez `http://cs:4000`.
- **Certificats CA et OCSP** : Les certificats sont situés dans le répertoire `/home/ca/CA/certs` du conteneur `cs`.
- **Ajout d'un répondeur OCSP** : L'URL du répondeur OCSP est `http://cs:8888`.
- **Certificat TSA** : Situé dans le répertoire `/home/ca/CA/certs` du conteneur `cs`. L'URL du service de timestamping est `http://cs:8899`.
- **Nom DNS des serveurs de sécurité** : Utilisez `ss1`, `ss2`, et `ss3` respectivement pour chaque serveur de sécurité.

6. **Accès aux interfaces utilisateur**

Les interfaces utilisateur des différents composants sont accessibles aux adresses suivantes :

- **Serveur Central (cs)** : `https://localhost:4000`
- **CA de test (cs)** : `https://localhost:8888/testca/`
- **Serveur de sécurité 1 (ss1)** : `https://localhost:1000`
- **Serveur de sécurité 2 (ss2)** : `https://localhost:2000`
- **Serveur de sécurité 3 (ss3)** : `https://localhost:3000`

7. **Test de l'environnement**

Pour tester que l'environnement fonctionne comme prévu après la configuration, utilisez les ports suivants pour chaque serveur de sécurité :

- **ss1** : 1080 / 1443
- **ss2** : 2080 / 2443
- **ss3** : 3080 / 3443


8. **Considérations supplémentaires**

- Utiliser différents navigateurs pour accéder aux interfaces de différents composants pour éviter les problèmes de session.
- Vérifiez le type de connexion du sous-système client avant d'invoquer un service, et mettez à jour la configuration si nécessaire pour éviter les erreurs.



---

Suite Partie 2