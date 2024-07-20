# Phase 1 : Sécurisation des Données dans Amazon S3

![image](https://github.com/user-attachments/assets/a41eef47-e1b9-436f-8656-fae2fb0dae92)

# Objectifs
Cette phase a pour but de sécuriser les données d'identification personnelle (PII) stockées dans Amazon S3 pour le compte du service financier de la Société X. Face à des violations de données récentes dans l'industrie, il est crucial de mettre en œuvre des mesures strictes pour protéger les données contre les accès non autorisés.

# Exigences
- **Limiter l'accès** aux compartiments S3 à certains gestionnaires de compte.
- **Activer la gestion des versions** pour tous les objets dans les compartiments S3.
- **Activer la journalisation des accès** pour tous les compartiments.
- **Chiffrer les compartiments** avec des clés gérées par Amazon S3 (SSE-S3).
- **Implémenter Amazon S3 Inventory** pour un suivi permanent des fichiers.
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Tâche 1.1 : Création d'un Compartiment et Application d'une Politique
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

### Étape 1 : Création d'un Compartiment S3
- **Connexion à AWS :** Connectez-vous à la console AWS et accédez au service Amazon S3.
- **Création du compartiment :** 
  - Nom du compartiment : `data-bucket-<unique-ID>`
  - Région : US East (N. Virginia)
  - Paramètres : Activez le chiffrement SSE-S3 par défaut pour le chiffrement des données au repos.

### Étape 2 : Chargement d'un Objet de Test
- **Préparation :** Créez un fichier texte nommé `myfile.txt` avec le contenu "hello world".
- **Upload :** Téléchargez ce fichier dans `data-bucket-<unique-ID>`.

### Étape 3 : Définition et Application de la Politique de Compartiment
- **Configuration de la politique :**
  ```json
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Principal": "*",
              "Action": "s3:*",
              "Resource": [
                  "arn:aws:s3:::data-bucket-<unique-ID>",
                  "arn:aws:s3:::data-bucket-<unique-ID>/*"
              ],
              "Condition": {
                  "StringEquals": {
                      "aws:PrincipalArn": [
                          "arn:aws:iam::<ACCOUNT_ID>:user/paulo",
                          "arn:aws:iam::<ACCOUNT_ID>:role/voclabs"
                      ]
                  }
              }
          },
          {
              "Effect": "Deny",
              "Principal": "*",
              "Action": "s3:*",
              "Resource": [
                  "arn:aws:s3:::data-bucket-<unique-ID>",
                  "arn:aws:s3:::data-bucket-<unique-ID>/*"
              ],
              "Condition": {
                  "StringNotEquals": {
                      "aws:PrincipalArn": [
                          "arn:aws:iam::<ACCOUNT_ID>:user/paulo",
                          "arn:aws:iam::<ACCOUNT_ID>:role/voclabs"
                      ]
                  }
              }
          }
      ]
  }
  ```
- **Appliquer la politique :** Accédez à l'onglet Permissions du compartiment et insérez cette politique.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Tâche 1.2 : Activation de la Gestion des Versions et Journalisation
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

### Gestion des Versions
- **Activation :** Activez la gestion des versions dans les propriétés du compartiment `data-bucket-<unique-ID>`.

### Journalisation des Accès
- **Configuration :** 
  - Créez un compartiment `s3-objects-access-log` pour les journaux.
  - Activez la journalisation dans les propriétés de `data-bucket-<unique-ID>`, spécifiez `s3-objects-access-log` pour stocker les journaux avec `/data-bucket` comme préfixe.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Tâche 1.3 : Configuration de S3 Inventory
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

### Configuration
- **Activation :** Dans l'onglet Management de `data-bucket-<unique-ID>`, configurez S3 Inventory.
- **Paramètres :**
  - Nom : `Inventory`
  - Destination : `s3-inventory`
  - Format de sortie : Apache Parquet
  - Options : Incluez toutes les versions des objets.

## Tests d'Accès
- **Test de Paulo :** Assurez-vous qu'il peut accéder et télécharger des fichiers depuis `data-bucket-<unique-ID>`.
- **Test de Mary :** Elle ne doit pas pouvoir accéder au compartiment ou voir son contenu, vérifiant ainsi l'efficacité de la politique de sécurité.

## Évaluation des Coûts
Évaluez l'impact financier de ces configurations en utilisant les pages de tarification d'Amazon S3 et Amazon Athena. Prenez en compte les coûts des requêtes, du stockage des journaux et de l'utilisation d'Athena pour les analyses.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Tâche 1.4 : Vérification du Fonctionnement de la Gestion des Versions
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

#### Configuration et Test
- **Upload Initial :**
  - Créez et téléchargez un fichier `customers.csv` contenant des informations de base de clients.
- **Vérification des Versions :**
  - Accédez à la console Amazon S3, ouvrez `data-bucket-<unique-ID>`, et vérifiez les versions de `customers.csv` sous l'onglet Versions.
- **Modification et Re-Téléchargement :**
  - Modifiez `customers.csv` en ajoutant de nouvelles entrées, puis téléchargez à nouveau pour créer une nouvelle version.
- **Vérification des Modifications :**
  - Dans la console S3, ouvrez les différentes versions de `customers.csv` pour confirmer que les modifications ont été enregistrées correctement.

#### Test d'Accès
- **Utilisateur Paulo :** Doit pouvoir accéder et télécharger toutes les versions.
- **Utilisatrice Mary :** Doit être bloquée, confirmant ainsi l'efficacité des restrictions d'accès.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Tâche 1.5 : Confirmation de la Journalisation au Niveau Objet et Utilisation d'Athena
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
#### Configuration de la Journalisation
- **Activation de la Journalisation :** Assurez-vous que la journalisation des objets est activée pour `data-bucket-<unique-ID>`, dirigeant les logs vers `s3-objects-access-log`.

#### Utilisation d'Athena pour Analyser les Logs
- **Création de la Table Athena :**
  - Dans Amazon Athena, créez une table externe nommée `bucket_logs` utilisant le format de données des logs S3. Utilisez la requête SQL suivante pour créer la table :
    ```sql
    CREATE EXTERNAL TABLE `default.bucket_logs`(
      `bucketowner` STRING,
      `bucket_name` STRING,
      `requestdatetime` STRING,
      `remoteip` STRING,
      `requester` STRING,
      `requestid` STRING,
      `operation` STRING,
      `key` STRING,
      `request_uri` STRING,
      `httpstatus` STRING,
      `errorcode` STRING,
      `bytessent` BIGINT,
      `objectsize` BIGINT,
      `totaltime` STRING,
      `turnaroundtime` STRING,
      `referrer` STRING,
      `useragent` STRING,
      `versionid` STRING,
      `hostid` STRING,
      `sigv` STRING,
      `ciphersuite` STRING,
      `authtype` STRING,
      `endpoint` STRING,
      `tlsversion` STRING,
      `accesspointarn` STRING,
      `aclrequired` STRING
    )
    STORED AS INPUTFORMAT
      'org.apache.hadoop.mapred.TextInputFormat'
    OUTPUTFORMAT
      'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    LOCATION
      's3://s3-objects-access-log-<UNIQUE-ID>/'
    ```
    - Assurez-vous de remplacer `<UNIQUE-ID>` avec l'ID unique de votre compartiment de logs.
- **Analyse des Logs :**
  - Exécutez des requêtes pour examiner les logs d'accès. Utilisez par exemple :
    ```sql
    SELECT requester, operation, key, httpstatus
    FROM "default"."bucket_logs"
    WHERE requester LIKE 'arn:aws:iam%';
    ```
  - Cette requête aide à filtrer et examiner les actions spécifiques des utilisateurs, illustrant comment Paulo peut accéder aux objets tandis que Mary reçoit des erreurs HTTP 403.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Tâche 1.6 : Examen du Rapport S3 Inventory avec S3 Select
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

#### Utilisation de S3 Select
- **Accès au Rapport Inventory :**
  - Dans la console Amazon S3, accédez à la configuration d'inventaire sous l'onglet Gestion de `data-bucket-<unique-ID>` et cliquez sur le lien de destination.
  - Utilisez S3 Select pour interroger le fichier inventory parquet pour visualiser les détails des objets stockés.
- **Analyse :**
  - Examinez les entrées pour vérifier l'exactitude des données d'inventaire, ce qui est crucial pour les audits de conformité et la gestion des données.

### Évaluation des Coûts
Analysez les coûts associés à l'utilisation des fonctionnalités activées dans cette phase, notamment :
- **Stockage S3 :** Coûts pour le stockage des données, des versions supplémentaires et des fichiers de logs.
- **Requêtes Athena :** Coûts pour les requêtes exécutées pour analyser les logs d'accès.
- **S3 Inventory :** Frais pour la génération de rapports d'inventaire.

Utilisez les pages de tarification d'Amazon S3 et Athena pour estimer ces coûts. Assurez-vous de prendre en compte les opérations de lecture et d'écriture qui peuvent augmenter les frais globaux.
