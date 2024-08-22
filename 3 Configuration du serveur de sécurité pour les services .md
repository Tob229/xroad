### Configuration du Serveur de Sécurité

#### 3.1 Configuration Initiale

**Accéder à l'Interface d'Administration :**
- L'interface d'administration du serveur de sécurité est accessible à l'adresse suivante : `https://localhost:1000/`.
- Lors de la première connexion, acceptez le certificat autosigné.
- Si les services d'arrière-plan ne sont pas encore démarrés, un message d'erreur peut apparaître. Attendez que les services soient opérationnels, puis connectez-vous en utilisant les identifiants administrateur créés lors de l'installation.

**Télécharger et Importer l'Ancre de Configuration :**
- Téléchargez l'ancre de configuration depuis l'interface d'administration du serveur central sous **Settings** -> **Internal Configuration and Anchor**.
- Importez l'ancre sur le serveur de sécurité et confirmez le téléchargement en vérifiant le hachage généré.

**Configuration du Membre Propriétaire :**
- Renseignez les informations requises :
  - **Member Name** : Rempli automatiquement après l'ajout du **Member Code**.
  - **Member Class** : Catégorie de l'organisation propriétaire.
  - **Member Code** : Code de l'organisation propriétaire.
  - **Security Server Code** : Code unique du serveur.
- Cliquez sur **Continue** pour enregistrer la configuration.

#### 3.2 Saisir du Code PIN

**Entrer le Code PIN :**
- Après la configuration initiale, le serveur de sécurité demandera un code PIN pour sécuriser les clés secrètes.
- Accédez à **SIGN and AUTH Keys** sous **TOKEN: SOFTTOKEN-0**.
- Entrez le code PIN et cliquez sur **Connect**. La barre de message d'erreur rouge disparaîtra.

#### 3.3 Configuration du Service d'Horodatage

**Configurer le Service de Temporographie :**
- Accédez à **System Parameters**.
- Sélectionnez un service d'horodatage dans la liste et cliquez sur **Add**.

#### 3.4 Création des Demandes de Certificat

**Créer des Demandes de Certificat :**
- Ouvrez la vue **Keys and Certificates** et sélectionnez **TOKEN: SOFTTOKEN-0**.
- Cliquez sur **Add Key**, attribuez-lui une étiquette facultative, et suivez les étapes pour générer la demande de certificat (CSR).
- Choisissez le type de certificat (**SIGN** pour la signature, **AUTH** pour l'authentification) et spécifiez les détails requis (client, service de certification, format RSE, etc.).
- La demande de certificat sera téléchargée dans le dossier de téléchargement de votre navigateur.

#### 3.5 Importation des Certificats

**Importer les Certificats :**
- Importez d'abord le certificat d'authentification en utilisant l'option **Import Certificates**. Sélectionnez le fichier du certificat et cliquez sur **Open**.
- Activez le certificat d'authentification après l'importation.
- Répétez l'opération pour le certificat de signature.

#### 3.6 Enregistrement du Certificat d'Authentification

**Enregistrer le Certificat d'Authentification :**
- Enregistrez le certificat en entrant le FQDN du serveur de sécurité et cliquez sur **Add**.
- Accédez à l'interface d'administration du serveur central pour approuver la demande de gestion en attente.
- Modifiez le fichier de configuration `/etc/xroad/conf.d/local.ini` pour activer l'approbation automatique des demandes de gestion 

```bash
docker exec -ti cs bash
apt install nano 
nano /etc/xroad/conf.d/local.ini
```
- Ajouter le contenu suivant dans le fichier /etc/xroad/conf.d/local.ini

```bash
[center]
auto-approve-auth-cert-reg-requests=true
auto-approve-client-reg-requests=true
auto-approve-owner-change-requests=true
```
- Redémarrez le service `xroad-center` pour appliquer les modifications.

```bash
docker restart cs 
```

#### 3.7 Ajouter un Sous-système pour les Services de Gestion

**Ajouter le Sous-système GESTION :**
- Cliquez sur **Add Subsystem** et sélectionnez l'organisation propriétaire du serveur de sécurité, puis entrez le code du sous-système **GESTION**.
- Si la demande d'enregistrement échoue, configurez d'abord les services de gestion dans l'interface d'administration du serveur central.

#### 3.8 Configuration des Services de Gestion

**Configurer les Services de Gestion :**
- Sélectionnez le sous-système **GESTION** et ouvrez l'onglet **Services**.
- Collez l'adresse **WSDL** copiée du serveur central et ajoutez-la.
- Activez le **WSDL** et configurez les services individuels en spécifiant les URLs et autres paramètres nécessaires.

#### 3.9 Tester les Services de Gestion

**Tester et Vérifier :**
- Testez les services de gestion configurés pour vous assurer qu'ils fonctionnent correctement. Vérifiez que tous les services sont activés et fonctionnent comme prévu.


___
Pour la configuration des autres serveurs de sécurité, veuillez reprendre le même procédé. 


Suite en Partie 4.

<img src="chemin/vers/image.png" alt="Description de l'image" width="200" height="100">
