# AWS KMS

- AWS Key Management Service (AWS KMS) est un service géré qui facilite la création et la gestion des clés de chiffrement utilisées pour sécuriser vos données. Voici un aperçu des concepts clés et des fonctions d'AWS KMS, ainsi que des détails sur la manière dont il est utilisé pour chiffrer les données au repos et en transit.

### 1. **Qu'est-ce qu'AWS KMS ?**
AWS KMS permet aux clients de créer, gérer et utiliser des clés de chiffrement. Les clés peuvent être utilisées pour chiffrer et déchiffrer des données dans divers services AWS tels que S3, EBS, RDS, et Redshift. Les clés gérées par AWS KMS peuvent être soit gérées par le client (CMKs), soit gérées par AWS.

### 2. **Données au repos vs Données en transit**
- **Données au repos** : Se réfère aux données stockées sur un disque ou dans un stockage cloud comme Amazon S3 ou EBS. Le chiffrement des données au repos protège vos données contre l'accès non autorisé en cas de compromission physique ou système.
- **Données en transit** : Se réfère aux données qui sont en mouvement, comme lorsqu'elles sont transmises sur le réseau. Le chiffrement des données en transit protège contre l'interception des données par des parties non autorisées.

### 3. **Utilisation de AWS KMS pour chiffrer les données au repos**
Dans le cadre du lab, vous allez utiliser AWS KMS pour:
- Créer une clé KMS gérée par le client.
- Utiliser cette clé pour chiffrer des objets dans Amazon S3 et des volumes Amazon EBS.
- Examiner comment AWS CloudTrail enregistre l'utilisation de la clé KMS, ce qui offre une traçabilité pour les activités d'accès et de gestion des clés.

### 4. **Exemples d'application du lab**
- **Amazon S3** : Vous allez chiffrer un objet stocké dans S3 en utilisant le chiffrement côté serveur avec AWS KMS (SSE-KMS). Lorsque vous stockez un objet, Amazon S3 demandera une clé de données à AWS KMS, qui sera utilisée pour chiffrer l'objet.
- **Amazon EBS** : Vous allez créer une copie chiffrée d'un volume EBS à partir d'un snapshot non chiffré. Cela implique de demander à AWS KMS de générer une clé de chiffrement pour sécuriser le volume.

### 5. **Sécurité et gestion des clés**
- AWS KMS permet de désactiver et de réactiver les clés, ce qui affecte l'accès aux données chiffrées avec ces clés. Par exemple, si vous désactivez une clé utilisée pour chiffrer un volume EBS, vous ne pourrez pas démarrer une instance EC2 utilisant ce volume tant que la clé n'est pas réactivée.

### 6. **Monitoring et conformité**
- AWS CloudTrail aide à surveiller et à enregistrer les appels d'API pour AWS KMS, vous permettant de voir quand et par qui les clés sont utilisées. Cela est crucial pour la conformité et la sécurité, permettant de détecter les utilisations anormales ou non autorisées des clés de chiffrement.

### Conclusion
AWS KMS joue un rôle vital dans la stratégie de sécurité d'AWS, permettant un chiffrement robuste des données au repos et en transit tout en offrant une gestion et un suivi centralisés des clés de chiffrement. Utiliser AWS KMS dans vos applications AWS assure que vos données sensibles sont protégées conformément aux meilleures pratiques de sécurité.
