### 2. Configuration du serveur central

#### 2.1 Enregistrement dans l'interface d'administration et création de la configuration initiale

Après l'installation du serveur central, l'interface d'administration peut être trouvée à l'adresse suivante : `https://localhost:4000`. Lors de la première connexion, le certificat auto-signé du serveur devra être accepté.

Si vous accédez à l'URL immédiatement après l'installation, il est possible que les services d'arrière-plan ne soient pas encore démarrés, ce qui peut entraîner un message d'erreur. Patientez un moment, puis reconnectez-vous à l'interface d'administration.

1. **Saisissez les identifiants de racine** d'interface d'administration créés pendant l'installation.
2. **Cliquez sur "Connect in"**.

Dans l'écran de configuration initiale, vous devrez renseigner les informations suivantes :

- **Identificateur d'instance**
- **Adresse centrale de service**
- **Code PIN** qui protège les clés secrètes du serveur.

Après avoir rempli les informations demandées, cliquez sur **"Submit"**. La configuration initiale est alors sauvegardée et un message confirme l'initialisation du serveur central. Vous pouvez maintenant poursuivre avec la configuration complète.

Au début, l'interface d'administration peut afficher plusieurs messages d'erreur, mais ceux-ci disparaîtront au fur et à mesure de la configuration. À ce stade, ces erreurs peuvent être ignorées.

#### 2.2 Ajout des classes de membres

1. Choisissez **"Settings"** > **"System settings"**.
2. **Add** une classe de membre appropriée pour l'organisation administrative et cliquez sur **"Save"**.

La classe de membre ajoutée apparaîtra dans la liste.

#### 2.3 Ajout d'un service de certification

Des informations sur les exigences techniques des prestataires de services de confiance sont disponibles ici.

Pour utiliser un service de certification (CA) de test, référez-vous aux emplacements du certificat racine de l'AC, du certificat OCSP, et de l'URL OCSP. Pour copier les fichiers depuis le conteneur central serveur, utilisez les commandes suivantes :

```bash
docker exec -ti cs /home/ca/CA/certs .
```

Ensuite, vérifiez les fichiers avec la commande :

```bash
ls certs
```

Enfin, renommez les certificats avec la commande suivante :

```bash
mv certs/tsa.cert.pem certs/tsa.pem
mv certs/ca.cert.pem certs/ca.pem
mv certs/ocsp.cert.pem certs/ocsp.pem
```

1. Dans l'interface web, choisissez **"Trust Services"** > **"Add certification service"**.
2. Téléchargez le certificat racine du service de certification et appuyez sur **"Upload"**.
3. Pour le test CA, définissez la valeur `CertificateProfileInfo` à : `ee.ria.xroad.common.certificateprofile.impl.FiVRKCertificateProfileInfoProvider`.

De plus amples informations sur les profils de certificats sont disponibles ici.

4. Cliquez sur **"Save"**.

Ensuite, ouvrez la vue détaillée du nouveau service de certification en cliquant sur le nom du service.

1. Choisissez **"OCSP Responders"**.
2. Entrez l'URL de l'OCSP responder.
3. Téléchargez le certificat de répondeur OCSP.
4. Cliquez sur **"Save"**.

L'interface des services de confiance après l'ajout du service de certification.

#### 2.4 Création d'un service de horodatage

Des informations sur les exigences techniques des prestataires de services de confiance sont disponibles ici.

1. Dans l'interface web, choisissez **"Trust Services"** > **"Timestamping services"** > **"Add timestamping service"**.
2. Ajoutez l'URL du service de timestamping.
3. Téléchargez le certificat du service d'horodatage.
4. Cliquez sur **"Add"**.

L'écran après avoir ajouté le service de timestamping.

#### 2.5 Ajout de l'organisation et du sous-système administratifs

1. Choisissez **"Members"**.
2. Entrez le **Nom du membre** - le nom de l'organisation administrative.
3. Sélectionnez la **Member class** dans la liste.
4. Entrez le **Member code** - identificateur d'organisation, par exemple ID d'entreprise.
5. Cliquez sur **"OK"**.

L'écran après l'ajout de l'organisation.

Ensuite, ouvrez les coordonnées de l'organisation en cliquant sur son nom.

1. Pour les services de gestion, ajoutez un sous-système à l'organisation en choisissant **"Subsystems"** > **"Add new subsystem"**.
2. Entrez le **Subsystem code** - le nom du sous-système.
3. Cliquez sur **"Add"**.

L'écran après l'ajout du sous-système.

#### 2.6 Configuration des services de gestion

1. Choisissez **"Settings"** > **"System settings"** > **"Management services"**.
2. Dans **"Service provider identifier"**, cliquez sur **"Edit"**.
3. Choisissez le sous-système qui a été ajouté plus tôt.
4. Cliquez sur **"Select"**.

L'écran après la configuration du sous-système pour les services de gestion.

#### 2.7 Ajout des clés de signature

1. Choisissez **"Global configuration"** > **"Internal configuration"** > **"Signing keys"**.
2. Saisissez le **Code PIN** choisi plus tôt.
3. Cliquez sur **"Connect in"**.
4. Choisissez **"Add key"**.
5. Définissez un **Nom amical** pour la clé.
6. Cliquez sur **"Add"**.

L'écran après avoir ajouté la clé de signature interne.

Ensuite, pour les clés de signature externe :

1. Choisissez **"Global configuration"** > **"External configuration"** > **"Signing keys"** > **"Add key"**.
2. Définissez un **Nom amical** pour la clé.
3. Cliquez sur **"Add"**.

L'écran après avoir ajouté la clé de signature externe.

Les messages d'erreur de l'interface utilisateur disparaîtront après un court délai. La configuration du serveur central est maintenant terminée.

___
Suite Partie 3