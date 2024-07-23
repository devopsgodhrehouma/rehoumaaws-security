# Laboratoire 6.1 : Surveillance et Alertes avec CloudTrail et CloudWatch

# https://www.youtube.com/playlist?list=PLu7OpmBCWRDfja-6NsoVIOX_X3LCzJSJq
# https://www.youtube.com/playlist?list=PLVQzKSZl3hU5i4edsBByq_kmGhKfkd75q

# Aperçu et objectifs du laboratoire

Dans ce laboratoire, vous allez configurer la journalisation et la surveillance dans un compte AWS. Vous allez apprendre à créer une piste AWS CloudTrail, qui sera un journal d'audit des appels d'API effectués dans le compte. Vous allez ensuite créer un sujet Amazon Simple Notification Service (Amazon SNS). En vous abonnant à ce sujet par email, vous serez alerté lorsque des événements particuliers se produisent. Ensuite, vous définirez une règle Amazon EventBridge. La règle détectera chaque fois que quelqu'un modifie un groupe de sécurité et vous enverra une alerte par email à ce sujet. Enfin, vous créerez une alarme Amazon CloudWatch pour détecter chaque fois que plusieurs tentatives de connexion échouent pour la console de gestion AWS.

Après avoir terminé ce laboratoire, vous devriez être capable de :

- Analyser les détails des événements dans l'historique des événements CloudTrail.
- Créer une piste CloudTrail avec la journalisation CloudWatch activée.
- Créer un sujet SNS et s'y abonner par email.
- Configurer une règle EventBridge pour surveiller les modifications des ressources dans un compte AWS.
- Créer des filtres de métriques CloudWatch et des alarmes CloudWatch.
- Interroger les journaux CloudTrail en utilisant CloudWatch Logs Insights.

## Durée

Ce laboratoire nécessitera environ 75 minutes pour être complété.

## Restrictions des services AWS

Dans cet environnement de laboratoire, l'accès aux services et actions de services AWS peut être limité à ceux nécessaires pour compléter les instructions du laboratoire. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou de réaliser des actions en dehors de celles décrites dans ce laboratoire.

## Scénario

Le laboratoire commence avec une instance Amazon Elastic Compute Cloud (Amazon EC2) associée à un groupe de sécurité. Dans une des tâches du laboratoire, vous testerez si la modification du groupe de sécurité invoque correctement une règle et vous envoie une notification par email. Le laboratoire commence également avec une piste CloudTrail préconfigurée qui écrit dans CloudWatch Logs. Le laboratoire inclut également un utilisateur AWS Identity and Access Management (IAM) préconfiguré, que vous utiliserez pour tester les alertes pour les tentatives de connexion échouées à la console.

À la fin de la tâche 3, vous aurez créé et configuré plus de ressources dans le compte. L'environnement ressemblera au schéma d'architecture suivant :

### Architecture à la fin de la tâche 3

(Insérer le diagramme ici)

À la fin de la tâche 5, vous aurez créé l'architecture illustrée dans le schéma suivant :

### Architecture à la fin de la tâche 5

(Insérer le diagramme ici)

## Accéder à la console de gestion AWS

1. En haut de ces instructions, cliquez sur **Démarrer le laboratoire**.
   - La session de laboratoire commence.
   - Un minuteur s'affiche en haut de la page et indique le temps restant dans la session.
   - Conseil : Pour actualiser la durée de la session à tout moment, cliquez de nouveau sur **Démarrer le laboratoire** avant que le minuteur n'atteigne 0:00.
   - Avant de continuer, attendez que l'icône circulaire à droite du lien **AWS** dans le coin supérieur gauche devienne verte. Lorsque l'environnement de laboratoire est prêt, le panneau **Détails AWS** s'affichera également.
   
2. Pour vous connecter à la console de gestion AWS, cliquez sur le lien **AWS** dans le coin supérieur gauche, au-dessus de la fenêtre du terminal.
   - Un nouvel onglet du navigateur s'ouvre et vous connecte à la console.
   - Conseil : Si un nouvel onglet du navigateur ne s'ouvre pas, une bannière ou une icône se trouve généralement en haut de votre navigateur avec le message que votre navigateur empêche le site d'ouvrir des fenêtres pop-up. Cliquez sur la bannière ou l'icône, puis choisissez **Autoriser les pop-ups**.

## Tâche 1 : Créer une piste CloudTrail avec les journaux CloudWatch activés

Dans cette tâche, vous allez analyser le type d'informations disponibles dans l'historique des événements CloudTrail. Vous allez également créer une piste CloudTrail avec la journalisation CloudWatch activée.

1. Analyser les informations disponibles dans l'historique des événements CloudTrail.
   - Dans la console, dans la zone de recherche à droite de **Services**, recherchez et choisissez **CloudTrail** pour ouvrir la console CloudTrail.
   - Dans le volet de navigation, choisissez **Historique des événements**.
   - Dans le menu déroulant **Lecture seule** sous le titre de la section **Historique des événements**, choisissez **Source de l'événement**.
   - Dans la zone de recherche à droite de **Source de l'événement**, commencez à taper **cloudformation.amazonaws.com** et choisissez-le lorsqu'il apparaît.
   - Les événements dans l'historique des événements sont filtrés de manière à n'afficher que les événements de piste d'audit où la source de l'événement était AWS CloudFormation.
   - Choisissez l'événement **CreateStack** le plus récent.
   - L'enregistrement de l'événement de l'événement choisi s'affiche.
   - Analyse : L'événement **CreateStack** s'est produit lorsque vous avez commencé ce laboratoire. La pile a créé des ressources dans le compte. Remarquez que l'enregistrement inclut des détails tels que **userIdentity** pour la personne qui a effectué l'appel API, **eventTime**, et **awsRegion**. D'autres détails essentiels de l'enregistrement d'audit sont également fournis.
   - Note : L'historique des événements existe par défaut dans chaque Région. L'historique montre les événements des 90 derniers jours pour la Région que vous visualisez. Cette vue est limitée aux événements de gestion avec des appels d'API de création, de modification et de suppression, et de l'activité du compte. Pour maintenir un enregistrement de l'activité du compte qui dépasse 90 jours, y compris tous les événements de gestion avec l'option d'inclure des événements de données et une activité en lecture seule, vous devez configurer une piste CloudTrail. Vous allez le faire dans la prochaine étape.

2. Créer une piste CloudTrail avec les journaux CloudWatch activés.
   - Dans le volet de navigation, choisissez **Pistes**.
   - Choisissez **Créer une piste**.
   - Sur la page **Choisir les attributs de la piste**, configurez les éléments suivants :
     - Nom de la piste : Entrez **MyLabCloudTrail**
     - Emplacement de stockage : Choisissez **Créer un nouveau bucket S3** et acceptez le nom par défaut du bucket, qui inclut **aws-cloudtrail-logs**.
     - Chiffrement SSE-KMS des fichiers journaux : Décochez la case (pour désactiver cette option).
     - Journaux CloudWatch : Sélectionnez **Activé**.
     - Groupe de journaux : Choisissez **Nouveau** et acceptez le nom par défaut du groupe de journaux.
     - Rôle IAM : Choisissez **Existant**.
     - Nom du rôle : Choisissez **LabCloudTrailRole**.
     - Conservez les autres attributs de piste par défaut, puis choisissez **Suivant**.
   - Sur la page **Choisir les événements de journalisation**, configurez les éléments suivants :
     - Type d'événement : Conservez **Événements de gestion** sélectionné, et ne sélectionnez pas **Événements de données** ou **Événements d'informations**.
     - Activité API : Conservez **Lecture et écriture** sélectionné.
     - Choisissez **Suivant**.
   - Faites défiler jusqu'en bas de la page.
   - Important : C'est ici que vous complèteriez le processus pour créer la piste. Cependant, l'utilisateur avec lequel vous êtes connecté n'a pas les permissions nécessaires pour créer une piste CloudTrail avec les journaux CloudWatch activés. Cela est dû aux restrictions de sécurité imposées aux comptes AWS utilisés pour les laboratoires.
   - À la place, choisissez **Annuler**.
3. Analyser la piste CloudTrail existante.
   - Remarquez qu'une piste nommée **LabCloudTrail** existe déjà. Elle est configurée avec les mêmes paramètres que ceux que vous avez choisis à l'étape précédente, à l'exception de différences mineures telles que le nom du bucket Amazon Simple Storage Service (Amazon S3) et le nom du groupe de journaux.
   - Cette piste CloudTrail, avec la journalisation CloudWatch activée, est un composant essentiel des solutions de surveillance et d'alerte que vous allez construire dans le reste de ce laboratoire.

Félicitations ! Dans cette tâche, vous avez appris à accéder aux détails des événements dans l'historique des événements CloudTrail et à créer une piste CloudTrail.

## Tâche 2 : Créer un sujet SNS et s'y abonner

Amazon SNS est un service de messagerie entièrement géré pour la communication d'application à application (A2A) et d'application à personne (A2P). La fonctionnalité A2P offre la possibilité d'envoyer des messages aux utilisateurs à grande échelle via SMS, notification push mobile et email.

Dans cette tâche, vous allez créer un sujet SNS et abonner votre adresse email à ce sujet. Le sujet sera utilisé dans les tâches suivantes pour vous envoyer des alertes par email sur les activités importantes qui se produisent dans le compte AWS.

1. Créer un sujet SNS

.
   - Dans la zone de recherche à droite de **Services**, recherchez et choisissez **Simple Notification Service** pour ouvrir la console Amazon SNS.
   - Pour ouvrir le volet de navigation, cliquez sur l'icône du menu dans le coin supérieur gauche.
   - Dans le volet de navigation, choisissez **Sujets**.
   - Choisissez **Créer un sujet**, et configurez les éléments suivants :
     - Type : Choisissez **Standard**.
     - Nom : Entrez **MySNSTopic**
     - Développez la section **Politique d'accès - optionnel**.
     - Définir qui peut publier des messages au sujet : Choisissez **Tout le monde**.
     - Définir qui peut s'abonner à ce sujet : Choisissez **Tout le monde**.
     - En bas de la page, choisissez **Créer un sujet**.

2. Créer un abonnement email au sujet SNS.
   - Choisissez **Créer un abonnement**, et configurez les éléments suivants :
     - ARN du sujet : Remarquez que le numéro de ressource Amazon (ARN) du sujet que vous venez de créer est déjà rempli.
     - Protocole : Choisissez **Email**.
     - Point de terminaison : Entrez une adresse email où vous pouvez recevoir des emails pendant ce laboratoire.
     - Faites défiler jusqu'en bas de la page et choisissez **Créer un abonnement**.
3. Vérifiez votre email et confirmez l'abonnement.
   - Vérifiez votre email pour un message des notifications AWS.
   - Dans le corps de l'email, cliquez sur le lien **Confirmer l'abonnement**.
   - Une page web s'ouvre et affiche un message indiquant que l'abonnement a été confirmé avec succès.

Dans cette tâche, vous avez réussi à créer un sujet SNS et un abonnement email à ce sujet. Vous utiliserez cette configuration dans les prochaines tâches.

## Tâche 3 : Créer une règle EventBridge pour surveiller les groupes de sécurité

Dans cette tâche, vous allez créer une règle EventBridge. La règle détectera chaque fois que des modifications des règles entrantes sont apportées à un nouveau groupe de sécurité ou à un groupe de sécurité existant dans la même région de votre compte AWS. Chaque fois que les conditions de la règle sont remplies, la règle publiera un message au sujet SNS que vous avez créé.

1. Créer une règle pour surveiller les modifications des groupes de sécurité EC2.
   - Dans la console, dans la zone de recherche à droite de **Services**, recherchez et choisissez **Amazon EventBridge** pour ouvrir la console EventBridge.
   - Choisissez **Créer une règle**.
   - Dans l'écran **Définir les détails de la règle**, entrez les détails suivants :
     - Nom : **MonitorSecurityGroups**
     - Bus d'événements : **default**
     - Type de règle : **Règle avec un modèle d'événement**.
   - Choisissez **Suivant**.
   - Dans l'écran **Créer un modèle d'événement**, entrez les détails suivants :
     - Source de l'événement : **Événements AWS ou événements des partenaires EventBridge**
     - Laissez les paramètres par défaut de l'événement d'exemple.
     - Sous **Modèle d'événement**, choisissez **Modèles personnalisés (éditeur JSON)**.
     - Copiez et collez le code suivant dans le champ **Entrer l'événement JSON** :

       ```json
       {
         "source": ["aws.ec2"],
         "detail-type": ["AWS API Call via CloudTrail"],
         "detail": {
           "eventSource": ["ec2.amazonaws.com"],
           "eventName": ["AuthorizeSecurityGroupIngress", "ModifyNetworkInterfaceAttribute"]
         }
       }
       ```
   - Choisissez **Suivant**.
   - Important : Pour enregistrer des événements avec une valeur **detail-type** de **AWS API Call via CloudTrail**, une piste CloudTrail avec la journalisation activée est nécessaire. La piste créée pour vous remplit cette condition nécessaire.
   - Dans la section **Sélectionner les cibles**, configurez les éléments suivants pour la **Cible 1** :
     - Types de cibles : **Service AWS**
     - Sélectionner une cible : Choisissez **Sujet SNS**.
     - Sujet : Choisissez **MySNSTopic**.
     - Développez **Paramètres supplémentaires**.
     - Pour **Configurer l'entrée de la cible**, choisissez **Transformateur d'entrée**.
     - Choisissez **Configurer le transformateur d'entrée**.
     - Faites défiler jusqu'à la section **Transformateur d'entrée de la cible**.
     - Dans le champ **Chemin d'entrée** (première boîte), copiez et collez le code suivant :

       ```json
       {"name":"$.detail.requestParameters.groupId","source":"$.detail.eventName","time":"$.time","value":"$.detail"}
       ```

     - Dans le champ **Modèle** (deuxième boîte), copiez et collez le texte suivant :

       ```json
       "L'appel d'API <source> a été effectué contre le groupe de sécurité <name> le <time> avec les détails suivants : <value>"
       ```

       Analyse : Le **Chemin d'entrée** que vous définissez définit quatre variables : **name**, **source**, **time**, et **value**. Pour chaque variable, une valeur est définie en se référant aux données contenues dans la structure JSON des événements CloudTrail qui correspondent au modèle d'événement que vous avez également défini. Le **Modèle d'entrée** que vous définissez définit les informations qui seront transmises à la cible, dans ce cas un sujet SNS. Remarquez que le modèle inclut les noms des quatre variables définies dans le **Chemin d'entrée**.
     - Choisissez **Confirmer**, puis choisissez **Suivant**.
   - Dans l'écran **Configurer les balises**, choisissez **Suivant**.
   - Dans l'écran **Réviser et créer**, faites défiler jusqu'en bas et choisissez **Créer une règle**.

2. Tester la règle EventBridge en modifiant un groupe de sécurité associé à une instance EC2.
   - Dans la zone de recherche à droite de **Services**, recherchez et choisissez **EC2** pour ouvrir la console Amazon EC2.
   - Dans le volet de navigation, choisissez **Instances**.
   - Cochez la case pour **LabInstance**.
     - Cette instance a été créée pour vous lorsque vous avez commencé le laboratoire.
   - Dans le volet inférieur, choisissez l'onglet **Sécurité**.
   - Sous **Groupes de sécurité**, choisissez le lien pour le nom du groupe de sécurité contenant **LabSecurityGroup**.
     - Les détails de ce groupe de sécurité s'affichent.
   - Sur l'onglet **Règles entrantes**, choisissez **Modifier les règles entrantes**.
   - Choisissez **Ajouter une règle**, et configurez les éléments suivants :
     - Type : Choisissez **SSH**.
     - Source : Choisissez **Partout - IPv4**.
     - Choisissez **Enregistrer les règles**.

3. Vérifiez l'historique des événements CloudTrail.
   - Accédez à la console CloudTrail.
   - Dans le volet de navigation, choisissez **Historique des événements**.
   - Remarquez les entrées les plus récentes qui apparaissent. Un événement devrait ressembler à celui de la capture d'écran suivante.
     - (Insérer la capture d'écran ici)
     - Note : Si un événement **AuthorizeSecurityGroupIngress** n'est pas encore apparu, vous devrez peut-être attendre une minute ou deux, puis actualiser l'historique. Pour actualiser l'historique, choisissez l'icône d'actualisation.
   - Choisissez le lien **AuthorizeSecurityGroupIngress**. Dans l'enregistrement de l'événement, assurez-vous que **fromPort** et **toPort** affichent 22 et non 80.
   - Dans la section **Enregistrement de l'événement**, remarquez que les détails de cet événement correspondent à certains des détails que vous avez définis dans la règle EventBridge que vous avez créée il y a un moment. Plus précisément, les paires nom-valeur **"eventSource": "ec2.amazonaws.com"** et **"eventName": "AuthorizeSecurityGroupIngress"** dans l'événement correspondent au modèle d'événement que vous avez défini dans la règle. Par conséquent, cet événement devrait entraîner la publication d'un message au sujet SNS que vous avez créé.

4. Vérifiez la boîte de réception de l'adresse email que vous avez abonnée au sujet SNS.
   - Vous devriez avoir reçu un message des notifications AWS indiquant qu'un appel d'API **AuthorizeSecurityGroupIngress** a été effectué. L'appel d'API s'est produit lorsque vous avez modifié le groupe de sécurité.
     - Note : Rappelez-vous que vous avez abonné votre adresse email au sujet SNS, c'est pourquoi vous avez reçu l'email.

Dans cette tâche, vous avez réussi à créer une règle EventBridge pour surveiller les modifications des groupes de sécurité Amazon EC2 dans la région. Vous avez également testé que la modification d'un groupe de sécurité invoque la règle, qui publie ensuite un message au sujet SNS. Enfin, vous avez vérifié que vous avez reçu un email avec les détails de l'événement, car vous aviez préalablement abonné votre email au sujet.

## Tâche 4 : Créer une alarme CloudWatch basée sur un filtre de métriques

Jusqu'à présent dans ce laboratoire, vous avez utilisé CloudTrail et EventBridge pour vous alerter chaque fois que quelqu'un modifie les règles entrantes d'un groupe de sécurité dans une des

 régions de votre compte. Dans cette tâche, vous allez utiliser un autre service, CloudWatch, pour vous notifier lorsqu'un utilisateur échoue à se connecter à la console de gestion AWS un certain nombre de fois.

1. Créer un filtre de métriques CloudWatch.
   - Dans la zone de recherche à droite de **Services**, recherchez et choisissez **CloudWatch** pour ouvrir la console CloudWatch.
   - Dans le volet de navigation, développez **Logs**, puis choisissez **Groupes de journaux**.
   - Cochez la case pour **CloudTrailLogGroup**.
     - Note : Rappelez-vous que lorsque vous avez créé la piste CloudTrail, vous l'avez configurée pour créer ce groupe de journaux.
   - Choisissez **Actions > Créer un filtre de métriques**, puis configurez les éléments suivants :
     - Modèle de filtre : Copiez et collez le code suivant :

       ```json
       { ($.eventName = ConsoleLogin) && ($.errorMessage = "Failed authentication") }
       ```

     - Choisissez **Suivant**.
     - Nom du filtre : Entrez **ConsoleLoginErrors**.
     - Espace de noms des métriques : Entrez **CloudTrailMetrics**.
     - Nom de la métrique : Entrez **ConsoleLoginFailureCount**.
     - Valeur de la métrique : Entrez **1**.
     - En bas de la page, choisissez **Suivant**.
     - Choisissez **Créer un filtre de métriques**.

2. Créer une alarme CloudWatch basée sur le filtre de métriques.
   - Sur l'onglet **Filtres de métriques**, cochez la case à droite du filtre de métriques **ConsoleLoginErrors** que vous venez de créer.
   - Choisissez **Créer une alarme**.
     - Un nouvel onglet du navigateur s'ouvre.
   - Sur la page **Spécifier la métrique et les conditions**, dans la section **Conditions**, configurez les détails de l'alarme suivants :
     - Chaque fois que **ConsoleLoginFailureCount** est : Choisissez **Supérieur/Égal**.
     - à : Entrez **3**.
     - Observez les paramètres. Cette alarme sera invoquée chaque fois que la somme de la métrique **ConsoleLoginFailureCount** que vous avez définie est supérieure ou égale à 3 dans une période de 5 minutes.
     - Choisissez **Suivant**.
   - Sur la page **Configurer les actions**, configurez les éléments suivants :
     - Sélectionner un sujet SNS : Choisissez **Sélectionner un sujet existant**.
     - Envoyer une notification à : Choisissez **MySNSTopic**.
     - Choisissez **Suivant**.
   - Sur la page **Ajouter un nom et une description**, configurez les éléments suivants :
     - Nom de l'alarme : Entrez **FailedLogins**.
     - Choisissez **Suivant**.
   - Faites défiler jusqu'en bas de la page, et choisissez **Créer une alarme**.

3. Tester l'alarme CloudWatch en tentant de se connecter à la console avec des informations d'identification incorrectes au moins trois fois.
   - Dans la zone de recherche à droite de **Services**, recherchez et choisissez **IAM** pour ouvrir la console IAM.
   - Dans le volet de navigation, choisissez **Utilisateurs**.
   - Choisissez le lien pour le nom d'utilisateur de test.
   - Choisissez l'onglet **Identifiants de sécurité**, puis copiez le lien de connexion à la console.
   - Collez le lien copié dans un nouvel onglet du navigateur pour charger la page de connexion à la console.
   - Entrez les informations d'identification, y compris un mot de passe incorrect, et tentez de vous connecter. Répétez cela au moins trois fois :
     - Nom d'utilisateur IAM : Entrez **test**.
     - Mot de passe : **test**.
     - Choisissez **Se connecter**.
     - Note : Chaque fois que vous tentez de vous connecter, vous verrez un message indiquant que vos informations d'authentification sont incorrectes. C'est attendu !

4. Rétablir votre accès au compte AWS.
   - Fermez tous les onglets du navigateur où vous avez la console de gestion AWS ouverte.
   - Sur la page des instructions du laboratoire, choisissez le lien **AWS** au-dessus de ces instructions pour vous connecter à nouveau en tant qu'utilisateur **voclabs**.
     - Important : Vos tentatives de connexion à la console en tant qu'utilisateur de test ont effacé les informations d'authentification précédentes du cache de votre navigateur. Par conséquent, vous devez vous réauthentifier pour accéder à nouveau à la console.

5. Grapher la métrique que vous avez créée.
   - Accédez à la console CloudWatch.
   - Dans le volet de navigation, développez **Metrics**, puis choisissez **Toutes les métriques**.
   - Dans la section **Metrics**, sous **Espaces de noms personnalisés**, choisissez **CloudTrailMetrics**.
     - Note : Si **CloudTrailMetrics** n'apparaît pas encore, attendez que la notification SNS soit reçue.
   - Choisissez **Metrics sans dimensions**.
   - Choisissez **ConsoleLoginFailureCount** puis choisissez **Grapher cette métrique uniquement**.
   - Dans la zone de graphe en haut de la page, un petit point bleu devrait apparaître. Le point indique qu'un échec de connexion a été détecté.

6. Vérifiez l'état et les détails de l'alarme dans la console CloudWatch.
   - Dans le volet de navigation, développez **Alarms**, puis choisissez **Toutes les alarmes**.
   - L'état de l'alarme **FailedLogins** devrait être **In alarm**.
     - Note : Si l'alarme ne montre pas cet état, attendez une minute ou deux. Pour actualiser la page, choisissez l'icône d'actualisation.
     - Conseil : Pour savoir si l'alarme a été invoquée récemment, choisissez le lien pour le nom de l'alarme **FailedLogins**, puis choisissez l'onglet **Historique**.

7. Vérifiez la boîte de réception de l'adresse email que vous avez abonnée au sujet SNS.
   - Vous devriez avoir reçu un message concernant plusieurs tentatives de connexion échouées, avec un contenu similaire à l'image suivante.
     - (Insérer l'image ici)

## Tâche 5 : Interroger les journaux CloudTrail en utilisant CloudWatch Logs Insights

Dans cette dernière tâche du laboratoire, vous allez utiliser CloudWatch Logs Insights pour interroger les journaux CloudTrail.

CloudWatch Logs Insights vous permet de rechercher et d'analyser de manière interactive vos données de journaux dans Amazon CloudWatch Logs. Vous pouvez effectuer des requêtes pour vous aider à répondre plus efficacement et rapidement aux problèmes opérationnels.

1. Exécuter une requête CloudWatch Logs Insights.
   - Dans la console CloudWatch, dans le volet de navigation, choisissez **Logs Insights**.
   - Dans le menu déroulant **Sélectionner un groupe de journaux** sous la section **Logs Insights**, sélectionnez **CloudTrailLogGroup**.
   - Supprimez le contenu existant du champ de requête, puis copiez et collez le code suivant dans le champ de requête :

     ```sql
     filter eventSource="signin.amazonaws.com" and eventName="ConsoleLogin" and responseElements.ConsoleLogin="Failure"
     | stats count(*) as Total_Count by sourceIPAddress as Source_IP, errorMessage as Reason, awsRegion as AWS_Region, userIdentity.arn as IAM_Arn
     ```

   - Choisissez **Exécuter la requête**.
   - La sortie devrait ressembler à la capture d'écran suivante :
     - (Insérer la capture d'écran ici)

Félicitations ! Dans cette tâche, vous avez réussi à interroger les journaux CloudTrail en utilisant CloudWatch Logs Insights.

## Soumettre votre travail

1. Pour enregistrer vos progrès, choisissez **Soumettre** en haut de ces instructions.
   - Lorsque vous y êtes invité, choisissez **Oui**.
   - Après quelques minutes, le panneau des notes apparaît et vous montre combien de points vous avez obtenus pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez **Notes** en haut de ces instructions.
     - Conseil : Vous pouvez soumettre votre travail plusieurs fois. Après avoir modifié votre travail, choisissez **Soumettre** à nouveau. Votre dernière soumission est enregistrée pour ce laboratoire.
   - Pour trouver des commentaires détaillés sur votre travail, choisissez **Rapport de soumission**.

## Fin du laboratoire

1. En haut de cette page, choisissez **Terminer le laboratoire**, puis choisissez **Oui** pour confirmer que vous voulez terminer le laboratoire.
   - Un panneau de message indique que le laboratoire est en cours de terminaison.
   - Pour fermer le panneau, choisissez **Fermer** dans le coin supérieur droit.

