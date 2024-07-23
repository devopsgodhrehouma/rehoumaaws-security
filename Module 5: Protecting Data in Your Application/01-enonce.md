**TP 5.1 : Chiffrement des données au repos à l'aide d'AWS KMS**

**Aperçu et objectifs du TP**

Dans ce TP, vous utiliserez le service de gestion des clés AWS (AWS KMS) pour chiffrer des données au repos. Vous créerez une clé AWS KMS, l'utiliserez pour chiffrer des objets stockés dans le service Amazon Simple Storage Service (Amazon S3) et pour chiffrer des volumes Amazon Elastic Block Store (Amazon EBS). Vous verrez également comment AWS CloudTrail fournit un journal d'audit de l'utilisation des clés AWS KMS et comment la désactivation de la clé affecte l'accès aux données.

Après avoir terminé ce TP, vous devriez être capable de :

- Créer une clé gérée par le client AWS KMS pour chiffrer et déchiffrer des données au repos.
- Stocker un objet chiffré dans un seau S3 en utilisant une clé de chiffrement.
- Tenter un accès public et un accès signé à un objet S3 chiffré.
- Surveiller l'utilisation de la clé de chiffrement en utilisant l'historique des événements CloudTrail.
- Chiffrer le volume racine d'une instance Amazon Elastic Compute Cloud (Amazon EC2) existante.
- Désactiver et réactiver une clé AWS KMS et observer les effets sur l'accès aux données.

**Durée**
Ce TP nécessitera environ 75 minutes pour être complété.

**Restrictions des services AWS**
Dans cet environnement de TP, l'accès aux services AWS et les actions de service peuvent être restreints aux nécessaires pour compléter les instructions du TP. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou de réaliser des actions au-delà de celles décrites dans ce TP.

**Scénario**
Le diagramme suivant montre l'architecture qui a été créée pour vous dans AWS au début du TP. Les ressources cloud d'AWS incluent un seau S3 vide nommé ImageBucket et une instance EC2 nommée LabInstance, à laquelle un volume EBS racine non chiffré est attaché.

À la fin de ce TP, vous aurez créé l'architecture montrée dans le diagramme suivant. Ce diagramme montre également certaines des actions que vous entreprendrez et les communications qui se produiront entre les services Amazon S3, Amazon EBS, AWS KMS et CloudTrail pendant le TP.

**Description du diagramme** : Utilisateur connecté aux ressources cloud d'AWS via Internet. ImageBucket contient un objet chiffré avec le chiffrement côté serveur avec AWS KMS, connu sous le nom de SSE-KMS. Le seau contient également une clé de données chiffrée, stockée comme métadonnées avec l'objet chiffré. Le volume EBS racine non chiffré a été détaché de LabInstance. Un instantané a été créé de ce volume, et l'instantané a été utilisé pour créer un volume EBS racine chiffré. Le volume chiffré est attaché à LabInstance. Les demandes de chiffrement ou de déchiffrement des objets S3 et des volumes EBS sont envoyées à AWS KMS, où la clé gérée par le client nommée MyKMSKey est stockée. CloudTrail enregistre l'utilisation de la clé AWS KMS sous forme d'événements. Fin de la description.

**Accès à la console de gestion AWS**
Au début de ces instructions, choisissez "Démarrer le TP".

La session du TP commence.

Un chronomètre s'affiche en haut de la page et indique le temps restant de la session.

**Astuce :** Pour rafraîchir la durée de la session à tout moment, choisissez "Démarrer le TP" de nouveau avant que le chronomètre atteigne 0:00.

Avant de continuer, attendez que l'icône circulaire à droite du lien AWS dans le coin supérieur gauche devienne verte. Lorsque l'environnement du TP est prêt, le panneau de détails AWS s'affichera également.

Pour vous connecter à la console de gestion AWS, choisissez le lien AWS dans le coin supérieur gauche, au-dessus de la fenêtre du terminal.

Un nouvel onglet de navigateur s'ouvre et vous connecte à la console.

**Astuce :** Si un nouvel onglet de navigateur ne s'ouvre pas, une bannière ou une icône est généralement en haut de votre navigateur avec le message que votre navigateur empêche l'ouverture de fenêtres pop-up. Choisissez la bannière ou l'icône, puis choisissez Autoriser les pop-ups.
---

**Tâche 1 : Création d'une clé AWS KMS**
Dans cette tâche, vous créerez une clé gérée par le client AWS KMS. La clé AWS KMS que vous créez sera utilisée plus tard dans le TP pour générer, chiffrer et déchiffrer des clés de données. Ces clés de données seront partagées avec d'autres services AWS, y compris Amazon S3 et Amazon EC2. Elles serviront à chiffrer les données réelles stockées dans un seau S3 et sur des volumes EBS.

- Créez une clé gérée par le client AWS KMS.
- Dans la console de gestion AWS, dans la boîte de recherche à droite de "Services", recherchez KMS et choisissez Key Management Service pour ouvrir la console AWS KMS.
- Dans le panneau de navigation, choisissez "Customer managed keys".
- Choisissez "Create key".
- Pour le type de clé, choisissez Symmetric.
- Note : Les clés symétriques ne quittent jamais AWS KMS non chiffrées. La clé que vous créez sera une clé secrète de 256 bits.
- Choisissez "Next".
- Pour l'Alias, entrez "MyKMSKey".
- Choisissez "Next".
- Pour les administrateurs de clé, recherchez et choisissez le rôle voclabs.
- Note : Les administrateurs de clé sont des utilisateurs ou des rôles qui géreront l'accès à la clé de chiffrement. Vous êtes actuellement connecté à la console avec le rôle voclabs.
- Choisissez "Next".
- Sur la page "Define key usage permissions", recherchez et choisissez à nouveau le rôle voclabs.
- Note : Les utilisateurs de clé sont les utilisateurs ou rôles qui peuvent utiliser la clé pour chiffrer et déchiffrer des données.
- Choisissez "Next".
- En bas de la page de révision, choisissez "Finish".
- Votre clé apparaît dans la liste des clés gérées par le client.
- Note : Si vous avez tenté ce TP au cours des 7 derniers jours, vous pourriez voir une clé plus ancienne ou des clés plus anciennes dans cette liste, avec le statut "Pending deletion". Le temps minimum qui doit s'écouler entre le moment où vous demandez qu'une clé soit programmée pour être supprimée et le moment où elle est effectivement supprimée est de 7 jours.
---
**Tâche 2 : Stockage d'un objet chiffré dans un seau S3**
Dans cette tâche, vous téléchargerez un fichier image dans un seau S3. Vous chiffrerez le fichier en utilisant la clé de chiffrement que vous avez créée lors de la tâche précédente.

- Pour télécharger le fichier nommé clock.png sur votre ordinateur, choisissez le lien suivant : Télécharger.
- Localisez le seau S3 et analysez ses paramètres de chiffrement.
- Dans la boîte de recherche à droite de "Services", recherchez et choisissez S3 pour ouvrir la console Amazon S3.
- Dans le panneau de navigation, choisissez "Buckets".
- Choisissez le lien pour le nom du seau qui contient imagebucket.
- Choisissez l'onglet "Properties".
- Dans la section "Default encryption", remarquez que le paramètre est actuellement activé.
- Lorsque ce paramètre est activé, les nouveaux objets stockés dans le seau sont automatiquement chiffrés. Ce paramètre est activé par défaut.

**Upload d'un fichier dans le seau et stockage en tant qu'objet chiffré :**
- En haut de la page, choisissez l'onglet "Objects".
- Choisissez "Upload".
- Choisissez "Add files".
- Parcourez et sélectionnez le fichier clock.png sur votre ordinateur.
- Développez la section "Properties", et configurez ce qui suit :
  - Dans la section "Server-side encryption settings", choisissez "Specify an encryption key".
  - Type de clé de chiffrement : Choisissez "AWS Key Management Service key (SSE-KMS)".
  - Clé AWS KMS : Sélectionnez "Choose from your AWS KMS keys".
  - Menu déroulant clé AWS KMS : Choisissez "MyKMSKey".
- En bas de la page, choisissez "Upload".
- Après que le téléchargement soit terminé, choisissez "Close" dans le coin supérieur droit.
- Remarquez que clock.png est maintenant répertorié comme un objet dans le seau.

**Analyse des paramètres de chiffrement sur l'objet :**
- Choisissez le lien pour l'objet clock.png.
- Dans l'onglet "Properties", trouvez la section "Server-side encryption settings".
- Remarquez que le chiffrement côté serveur utilisant SSE-KMS est activé sur cet objet.

Dans cette tâche, vous avez analysé les paramètres de chiffrement sur le seau. Vous avez ensuite téléchargé un objet dans le seau. Bien que les paramètres du seau n'encryptent pas automatiquement les objets que vous téléchargez, vous aviez l'option de chiffrer un objet que vous avez téléchargé.
---
**Tâche 3 : Tentative d'accès public à l'objet chiffré**

Dans cette tâche, vous tenterez d'accéder à l'objet chiffré en utilisant l'URL de l'objet S3. Cette URL est une méthode pour accéder à un objet S3 depuis l'internet public.

- Tentez d'ouvrir l'image en utilisant l'URL de l'objet S3.
- Dans la section "Object overview" en haut de la page, copiez l'URL de l'objet dans votre presse-papiers.
- Note : Remarquez que l'URL est au format suivant : https://.s3.amazonaws.com/.
- Collez l'URL de l'objet dans un nouvel onglet de navigateur et tentez de charger la page.
- Vous recevrez l'erreur suivante "Access Denied".

Note : L'URL de l'objet S3 est une méthode pour fournir un accès public à un objet S3 sur Internet. Cependant, par défaut, lorsque vous créez un seau S3, l'accès public aux objets n'est pas autorisé. Le paramètre de sécurité "Block all public access" est appliqué par défaut à ce seau et à tous les objets qu'il contient.

- Gardez l'onglet du navigateur ouvert, car vous y retournerez dans un instant. Retournez à l'onglet du navigateur où la console Amazon S3 est ouverte.

**Modifier les permissions d'accès public pour le seau :**
- Dans les fil d'Ariane, situés dans le coin supérieur gauche de la page, choisissez le nom du seau qui contient imagebucket.
- Choisissez l'onglet "Permissions".
- Dans la section "Block public access", choisissez "Edit".
- Décochez "Block all public access".
- Choisissez "Save changes".
- Lorsque cela vous est demandé, entrez "confirm" et choisissez "Confirm".

**Modifier les paramètres d'accès pour l'objet :**
- Choisissez le lien pour le nom du seau qui contient imagebucket et choisissez l'onglet "Permissions".
- Sous "Object Ownership" choisissez "edit". Choisissez "ACLs enabled".
- Sélectionnez la case à cocher pour l'acquittement. Gardez "Bucket owner preferred" sélectionné, et choisissez "Save changes".
- Choisissez l'onglet "Objects", puis sélectionnez "clock.png".
- Choisissez "Actions > Make public using ACL".
- Choisissez "Make public".
- Une bannière s'affiche pour indiquer que les paramètres d'accès public ont été modifiés avec succès.
- Choisissez "Close".

**Tester à nouveau l'accès à l'objet en utilisant l'URL de l'objet S3 :**
- Retournez à l'onglet du navigateur que vous aviez ouvert plus tôt et qui avait tenté de charger l'URL de l'objet S3.
- Rafraîchissez la page.
- Vous verrez désormais une erreur différente s'afficher. Au lieu de l'erreur "Access Denied" que vous aviez vue précédemment, vous verrez désormais l'erreur "Invalid Argument".

Analyse : L'accès à l'objet est maintenant accordé au public sur Internet (via l'URL de l'objet) ; cependant, comme l'image est chiffrée, vous n'êtes toujours pas capable de la visualiser. C'est une bonne chose, car cela démontre une raison importante pour laquelle vous pourriez choisir de chiffrer des données. Dans les cas où l'accès aux données pourrait être accidentellement accordé à quelqu'un qui ne devrait pas y avoir accès, la personne ne pourra pas lire les données à moins d'avoir également accès à la clé de chiffrement. La clé de chiffrement est nécessaire pour déchiffrer les données et les rendre à nouveau lisibles.

Le message d'erreur indique que les requêtes spécifiant un chiffrement côté serveur avec des clés gérées par AWS KMS nécessitent AWS Signature Version 4. La Signature Version 4 est le processus pour ajouter des informations d'authentification aux requêtes AWS.

- Fermez l'onglet du navigateur où vous avez chargé l'URL de l'objet S3.

Dans cette tâche, vous avez constaté comment l'objet chiffré stocké dans Amazon S3 n'était d'abord pas accessible via Internet. Puis, vous avez découvert que, même lorsque vous avez rendu l'objet disponible sur Internet, vous n'avez pas réussi à y accéder avec succès. Cela est arrivé parce que les requêtes spécifiant SSE-KMS nécessitent AWS Signature Version 4, et vous n'avez pas signé la requête.
---

**Tâche 4 : Tentative d'accès signé à l'objet chiffré**

Dans cette tâche, vous tenterez de déchiffrer l'objet chiffré depuis la console Amazon S3 en tant qu'utilisateur authentifié.

- Ouvrez l'objet chiffré stocké.
- Dans la console Amazon S3, assurez-vous d'être sur l'onglet "Objects" pour le seau contenant imagebucket.
- Sélectionnez "clock.png", puis choisissez "Open".
- L'image suivante s'ouvre dans un nouvel onglet ou une nouvelle fenêtre.

**Analyse de l'URL utilisée pour déchiffrer avec succès l'objet :**
- Remarquez l'URL dans l'onglet du navigateur où l'image est ouverte. Le format est similaire au suivant (où les valeurs ... contiennent des informations d'identification).

```
https://c46588a730572l1728517t1w501832962033-imagebucket-gmnhbh84muj.s3.us-east-1.amazonaws.com/clock.png?response-content-disposition=inline&X-Amz-...&X-Amz-...&X-Amz-...&X-Amz-...&X-Amz-...&X-Amz-...&X-Amz-...
```
- Fermez l'onglet du navigateur où l'image est chargée et retournez à la console Amazon S3.

**Explication des étapes qui ont eu lieu lors de la demande d'ouverture de l'objet chiffré :**
1. Vous avez demandé à ouvrir l'objet.
2. Ensuite, Amazon S3 a remarqué que l'objet demandé était chiffré. Comme vous étiez authentifié au compte AWS en utilisant la console Amazon S3, les informations d'authentification Signature Version 4 ont été automatiquement incluses dans la requête.
3. Amazon S3 a ensuite envoyé la copie chiffrée de la clé de données avec laquelle l'objet était chiffré à AWS KMS.
4. AWS KMS a alors déchiffré la clé de données en utilisant la clé AWS KMS MyKMSKey (qui ne quitte jamais le service AWS KMS).
5. AWS KMS a ensuite renvoyé la clé de données en clair à Amazon S3.
6. Finalement, Amazon S3 a déchiffré le texte chiffré de l'objet de données, vous a permis d'ouvrir l'objet et a supprimé la copie en clair de la clé de données.

---
**Tâche 5 : Surveillance de l'activité AWS KMS à l'aide de CloudTrail**

Dans cette tâche, vous accéderez à l'historique des événements CloudTrail pour trouver des événements liés à vos opérations de chiffrement. La fonctionnalité de journal d'audit CloudTrail offre une caractéristique de sécurité importante, et il est judicieux de surveiller comment les clés AWS KMS sont utilisées dans votre compte.

- Accédez à l'historique des événements CloudTrail.
- Dans la boîte de recherche à droite de "Services", recherchez et choisissez CloudTrail pour ouvrir la console CloudTrail.
- Dans le panneau de navigation, choisissez "Event history".
- Note : CloudTrail fournit un journal d'audit des appels API effectués dans le compte AWS. L'historique des événements donne accès aux événements des 90 derniers jours d'activité du compte.
- Remarquez la colonne nommée "Event source".

Chaque fois qu'un appel API à un service AWS se produit dans la région que vous avez sélectionnée, si ce service rapporte de tels événements à CloudTrail, alors l'événement et le service AWS qui a rapporté l'événement sont listés.

- Filtrez l'historique des événements pour afficher uniquement les événements que le service AWS KMS a rapportés.
- Choisissez le menu déroulant sur la gauche qui affiche actuellement "Read-only", et choisissez "Event source".
- Dans la boîte de recherche "Enter an event source", recherchez kms et choisissez kms.amazonaws.com.

**Analyse de l'événement GenerateDataKey :**
- Choisissez le lien pour le nom de l'événement GenerateDataKey.
- Dans la section "Event record", observez les détails de l'événement.
- Les détails sont similaires à l'image suivante. Dans cette image, les détails essentiels de l'enregistrement de l'événement sont mis en évidence.

**Enregistrement de l'événement GenerateDataKey. Les informations mises en évidence incluent principalId, eventName, keyId, et le nom de l'objet S3.**

Analyse : Cet événement a été généré lorsque vous avez demandé de télécharger le fichier clock.png sur Amazon S3. Votre action a incité AWS KMS à générer une nouvelle clé de données. Si vous allez à la console AWS KMS et regardez l'ID de clé de la clé AWS KMS que vous avez créée lors de la tâche 1, vous trouverez qu'elle correspond à l'ID de clé dans l'enregistrement de l'événement. Remarquez également que l'événement spécifie l'ARN de l'objet que Amazon S3 prévoyait de chiffrer avec la clé de données, après avoir reçu la clé de données de AWS KMS. Les détails de l'événement listent également l'identité (principalId) de l'utilisateur ou du rôle qui a effectué la demande.
**Analyse de l'événement Decrypt :**
- Retournez à l'historique des événements en choisissant "Event history" dans le fil d'Ariane en haut de la page.
- Les événements devraient toujours être filtrés par la source d'événement kms.amazonaws.com.
- Choisissez le lien pour le nom de l'événement "Decrypt".
- Comme précédemment, observez les détails dans la section "Event record".

Analyse : Cet événement a été généré lorsque vous avez réussi à ouvrir le fichier clock.png depuis la console Amazon S3. Notez que l'enregistrement détaille à nouveau qui a fait la demande, quelle clé AWS KMS a fourni la clé de données non chiffrée à Amazon S3, et quel objet S3 a été déchiffré avec la clé de données en clair.

CloudTrail enregistre toute l'activité de l'API AWS KMS. En évaluant ces entrées de journal, vous pourriez être en mesure de déterminer l'utilisation passée d'une clé AWS KMS particulière. Si vous souhaitez pouvoir analyser des événements survenus il y a plus de 90 jours, vous pouvez créer une piste CloudTrail.

---

**Tâche 6 : Chiffrement du volume racine d'une instance EC2 existante**

Dans cette tâche, vous allez chiffrer le volume racine d'une instance EC2 existante.

**Étapes nécessaires pour chiffrer un volume non chiffré :**
1. Arrêtez l'instance.
2. Détachez le volume.
3. Créez un instantané du volume racine non chiffré.
4. Créez un nouveau volume chiffré à partir de cet instantané.
5. Remplacez le volume racine par le nouveau volume chiffré.
6. Finalement, redémarrez l'instance.

**Observer les paramètres de stockage actuels sur une instance EC2 existante :**
- Dans la boîte de recherche à droite de "Services", recherchez et choisissez EC2 pour ouvrir la console Amazon EC2.
- Dans le panneau de navigation, choisissez "Instances".
- Choisissez le lien pour l'ID de l'instance LabInstance.
- Choisissez l'onglet "Storage".
- Dans la section "Block devices", remarquez que le volume attaché indique qu'il n'est pas chiffré. Il s'agit du volume racine, qui contient l'installation du système d'exploitation invité.
- Note : Vous pouvez chiffrer un volume attaché à une instance EC2 lors de sa création initiale. Vous pouvez également chiffrer un volume attaché à une instance EC2 existante, y compris le volume racine, mais cela nécessite quelques étapes supplémentaires. Vous compléterez ces étapes dans cette tâche.

**Arrêtez l'instance :**
- Dans le fil d'Ariane en haut de la page, choisissez "Instances".
- Sélectionnez LabInstance, et choisissez "Instance state > Stop instance".
- Pour confirmer l'action, choisissez "Stop".

**Créez un instantané du volume EBS racine de l'instance EC2 existante :**
- Choisissez l'onglet "Storage".
- Dans la section "Block devices", choisissez le lien pour l'ID du volume.
- Notez la zone de disponibilité où se trouve le volume (par exemple, us-east-1a ou us-east-1b).
- Important : Vous aurez besoin de ces informations dans un instant.
- Choisissez "Actions > Create snapshot".
- Choisissez "Add tag", et ajoutez une étiquette avec les informations suivantes :
  - Clé : Entrez "Name"
  - Valeur : Entrez "Unencrypted Root Volume"
- Choisissez "Create snapshot".

**Créez un volume chiffré à partir de l'instantané non chiffré :**
- Dans le panneau de navigation, sous "Elastic Block Store", choisissez "Snapshots".
- Choisissez le lien pour l'ID de l'instantané "Unencrypted Root Volume" que vous venez de créer.
- Attendez que le statut de l'instantané indique "Completed".
- Remarquez que le statut de chiffrement de l'instantané est "Not encrypted".
- Choisissez "Actions > Create volume from snapshot", et configurez ce qui suit :
  - Zone de disponibilité : Choisissez la zone de disponibilité où se trouve le volume existant.
  - Sélectionnez "Encrypt this volume".
  - Clé KMS : Choisissez "MyKMSKey".
- Choisissez "Create volume".

**Étiquetez les volumes :**
- Dans le panneau de navigation, sous "Elastic Block Store", choisissez "Volumes".
- Remarquez que deux volumes sont désormais listés.
- Pour le volume avec un état "In-use", changez le nom du volume :
  - Survolez le champ "Name", et choisissez l'icône de crayon et de papier.
  - Dans la boîte "Edit Name", entrez "Old unencrypted root volume"
- Choisissez "Save".
- Suivez les mêmes étapes pour changer le nom du volume avec un état "Available" en "New encrypted root volume"

**Échangez le volume racine que l'instance EC2 utilise :**
- Sélectionnez "Old unencrypted root volume", puis choisissez "Actions > Detach volume".
- Pour confirmer, choisissez "Detach".
- Sélectionnez "New encrypted root volume", puis choisissez "Actions > Attach volume" et configurez ce qui suit :
  - Instance : Choisissez (LabInstance) (arrêtée).
  - Nom du périphérique : Entrez "/dev/xvda"
- Note : C'est le nom du périphérique où l'instance existante s'attend à trouver le volume racine.
- Choisissez "Attach volume".

**Remarquez que le volume racine est maintenant chiffré.**
- Retournez à l'écran "Instances", et sélectionnez LabInstance.
- Choisissez l'onglet "Storage", et remarquez que le volume attaché est maintenant chiffré et possède un ID de clé AWS KMS.
- Note : Vous devrez peut-être rafraîchir la page pour voir les dernières informations concernant le volume attaché.
- Important : Ne démarrez pas encore l'instance. Vous le ferez lors de la tâche suivante.

Dans cette tâche, vous avez chiffré le volume racine d'une instance existante, rendant ainsi les données qui y sont stockées plus sécurisées.
---
**Tâche 7 : Désactivation de la clé de chiffrement et observation des effets**

Dans cette tâche, vous désactiverez temporairement la clé AWS KMS que vous avez utilisée précédemment pour chiffrer un objet dans Amazon S3 et un volume EBS. Vous observerez ensuite les effets de la désactivation de la clé sur l'accès aux données chiffrées.

**Désactiver la clé AWS KMS utilisée pour chiffrer l'objet S3 et le volume EBS :**
- Naviguez vers la console AWS KMS.
- Dans le panneau de navigation, choisissez "Customer managed keys".
- Sélectionnez "MyKMSKey", puis choisissez "Key actions > Disable".
- Pour confirmer l'action, sélectionnez "Confirm that you want to disable this key", puis choisissez "Disable key".

**Tenter de démarrer l'instance EC2 :**
- Naviguez vers la console Amazon EC2.
- Sélectionnez "LabInstance", puis choisissez "Instance state > Start instance".
- Utilisez l'icône de rafraîchissement pour surveiller l'état de l'instance.
- L'instance échouera rapidement à démarrer et l'état de l'instance passera de "Pending" à "Stopped" à nouveau.

**Tenter d'accéder à l'objet S3 en utilisant l'URL de l'objet :**
- Naviguez vers la console Amazon S3.
- Dans le panneau de navigation, choisissez "Buckets".
- Choisissez le lien pour le nom du seau qui contient "imagebucket".
- Sélectionnez "clock.png", puis choisissez "Open".
- L'image échouera à s'ouvrir, et une erreur "KMS.DisabledException" s'affichera, bien que cette méthode pour décrypter l'objet ait été réussie pour vous plus tôt dans le TP.

**Analyser l'historique des événements CloudTrail :**
- Naviguez vers la console CloudTrail.
- Dans le panneau de navigation, choisissez "Event history".
- Choisissez le lien pour le nom de l'événement "DisableKey".
- Examinez les détails de l'événement.
- Les détails montrent que vous avez désactivé la clé AWS KMS il y a quelques minutes.
- Examinez l'événement "StartInstances", qui s'est produit juste après l'événement "DisableKey".
- La demande de démarrage de l'instance a été réussie. Cependant, comme vous l'avez expérimenté, l'instance n'a jamais atteint l'état "Running".
- Examinez l'événement "CreateGrant", qui s'est produit juste après l'événement "StartInstances". L'enregistrement de l'événement est similaire au suivant :

**Enregistrement de l'événement CreateGrant. Le message d'erreur indique que la clé est désactivée.**

Remarquez que les détails de l'événement révèlent une erreur. Le processus de démarrage de l'instance EC2 a remarqué que le volume racine était chiffré avec "MyKMSKey", donc Amazon EC2 a contacté AWS KMS pour fournir la clé de données en clair afin de pouvoir déchiffrer le volume. Mais AWS KMS a refusé la demande parce que la clé AWS KMS utilisée pour chiffrer la clé de données avec laquelle le volume EBS était chiffré était désactivée. L'instance ne peut pas démarrer parce que les fichiers sur le volume racine dont le système d'exploitation invité a besoin pour démarrer n'ont pas pu être déchiffrés.

**Réactiver la clé AWS KMS gérée par le client MyKMSKey :**
- Retournez à la console AWS KMS et réactivez la clé "MyKMSKey".

**Redémarrer l'instance LabInstance :**
- Retournez à la console Amazon EC2 et démarrez à nouveau "LabInstance".
- L'instance démarre avec succès maintenant.

**Important : Avant de soumettre votre travail pour le TP, attendez que l'état de l'instance affiche "Running".**

**Soumission de votre travail**
- Pour enregistrer votre progression, choisissez "Submit" en haut de ces instructions.
- Lorsqu'on vous le demande, choisissez "Yes".
- Après quelques minutes, le panneau des notes apparaît et vous montre combien de points vous avez gagnés pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez "Grades" en haut de ces instructions.
- Astuce : Vous pouvez soumettre votre travail plusieurs fois. Après avoir modifié votre travail, choisissez "Submit" à nouveau. Votre dernière soumission est enregistrée pour ce TP.
- Pour trouver un retour détaillé sur votre travail, choisissez "Submission Report".

**TP terminé**
Félicitations ! Vous avez complété le TP.

- En haut de cette page, choisissez "End Lab", puis choisissez "Yes" pour confirmer que vous souhaitez terminer le TP.
- Un panneau de message indique que le TP est en train de se terminer.
- Pour fermer le panneau, choisissez "Close" dans le coin supérieur droit.

