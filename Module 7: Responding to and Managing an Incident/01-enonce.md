## Lab 7.1 : Remédier à un incident en utilisant AWS Config et Lambda

### Aperçu et objectifs du laboratoire

Dans ce laboratoire, vous apprendrez à utiliser le service AWS Config pour surveiller les modifications apportées à des ressources spécifiques dans votre compte AWS. Vous découvrirez comment utiliser ce service pour identifier des modifications pouvant poser des problèmes de sécurité, comme lorsqu'un utilisateur modifie un groupe de sécurité Amazon Elastic Compute Cloud (Amazon EC2). Ensuite, vous acquerrez une expérience pratique en intégrant AWS Config avec AWS Lambda pour remédier automatiquement à certains incidents de sécurité préoccupants.

À la fin de ce laboratoire, vous devriez être capable de :

- Expliquer comment utiliser les rôles AWS Identity and Access Management (IAM) pour accorder aux services AWS l'accès à d'autres services AWS.
- Activer AWS Config pour surveiller les ressources dans un compte AWS.
- Créer et activer une règle personnalisée AWS Config qui utilise une fonction Lambda pré-créée.
- Tester le comportement d'une règle AWS Config pour s'assurer qu'elle fonctionne comme prévu.
- Analyser les logs Amazon CloudWatch pour auditer les moments où les règles AWS Config sont invoquées.

### Durée
Ce laboratoire nécessite environ 75 minutes pour être complété.

### Restrictions des services AWS
Dans cet environnement de laboratoire, l'accès aux services AWS et aux actions des services peut être limité à ceux nécessaires pour suivre les instructions du laboratoire. Vous pouvez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou d'effectuer des actions au-delà de celles décrites dans ce laboratoire.

### Scénario
Pendant ce laboratoire, votre responsabilité est de surveiller les paramètres des groupes de sécurité Amazon EC2 dans un compte AWS. Vous définirez quels ports entrants doivent être ouverts ou fermés dans un groupe de sécurité. Vous configurerez une solution pour remédier automatiquement à un incident où quelqu'un modifie les règles entrantes d'un groupe de sécurité et qu'elles ne correspondent plus à la configuration souhaitée.

Lorsque vous commencerez le laboratoire, votre compte AWS contiendra deux rôles IAM et une fonction Lambda. Il contiendra également un VPC par défaut avec un groupe de sécurité par défaut et un VPC personnalisé nommé Lab VPC, qui a un groupe de sécurité nommé LabSG1.

Le diagramme suivant montre l'architecture créée pour vous dans AWS au début du laboratoire.

#### Architecture de départ

Pendant le laboratoire, vous configurerez le service AWS Config pour créer un inventaire des ressources spécifiques dans une région de votre compte AWS. Vous créerez ensuite une règle AWS Config.

À la fin de ce laboratoire, vous aurez créé l'architecture montrée dans le diagramme suivant.

#### Architecture finale

Après avoir construit la solution, un incident de sécurité sera remédié par les étapes décrites dans le tableau suivant.

| Étape | Explication |
|-------|-------------|
| 1     | La règle AWS Config surveillera toute modification des groupes de sécurité suivis dans l'inventaire des ressources AWS Config. |
| 2     | Lorsque la règle remarque des modifications apportées à un groupe de sécurité, elle invoquera la fonction Lambda. |
| 3     | La fonction remédiera à la situation en mettant à jour la configuration des règles entrantes souhaitées pour le groupe de sécurité. |

### Accéder à la console de gestion AWS
En haut de ces instructions, choisissez **Start Lab**.

La session de laboratoire commence.

Un minuteur s'affiche en haut de la page et montre le temps restant dans la session.

**Conseil :** Pour rafraîchir la durée de la session à tout moment, choisissez **Start Lab** à nouveau avant que le minuteur n'atteigne 0:00.

Avant de continuer, attendez que l'icône circulaire à droite du lien AWS dans le coin supérieur gauche devienne verte. Lorsque l'environnement de laboratoire est prêt, le panneau AWS Details s'affiche également.

Pour vous connecter à la console de gestion AWS, choisissez le lien AWS dans le coin supérieur gauche, au-dessus de la fenêtre de terminal.

Un nouvel onglet du navigateur s'ouvre et vous connecte à la console.

**Conseil :** Si un nouvel onglet du navigateur ne s'ouvre pas, une bannière ou une icône se trouve généralement en haut de votre navigateur avec le message que votre navigateur empêche le site d'ouvrir des fenêtres contextuelles. Choisissez la bannière ou l'icône, puis choisissez **Autoriser les fenêtres contextuelles**.

### Tâche 1 : Examiner et mettre à jour les rôles IAM
Dans cette tâche, vous analyserez deux rôles IAM pré-approvisionnés pour vous dans l'environnement de laboratoire. Vous mettrez également à jour les permissions de l'un des rôles. AWS Config et Lambda utiliseront ces rôles plus tard dans le laboratoire.

1. Dans la console IAM, observez les permissions accordées au rôle **AwsConfigLambdaSGRole**.
   - Dans la boîte de recherche à droite de **Services**, recherchez et choisissez **IAM**.
   - Dans le panneau de navigation, choisissez **Roles**.
   - Choisissez le lien **AwsConfigLambdaSGRole**.
   - Dans l'onglet **Permissions**, développez **awsconfig_lambda_ec2_sg_role_policy**.

Le document de politique IAM suivant s'affiche. La politique est formatée en JavaScript Object Notation (JSON).

```json
{
 "Version":"2012-10-17",
 "Statement":[
  {
     "Action":[
       "logs:CreateLogGroup",
       "logs:CreateLogStream",
       "logs:PutLogEvents"
     ],
     "Resource":"arn:aws:logs:*:*:*",
     "Effect":"Allow"
  },
  {
    "Action":[
       "config:PutEvaluations",
       "ec2:DescribeSecurityGroups",
       "ec2:AuthorizeSecurityGroupIngress",
       "ec2:RevokeSecurityGroupIngress"
    ],
    "Resource":"*",
    "Effect":"Allow"
  }
 ]
}
```

**Analyse :** Il s'agit d'un rôle personnalisé créé pour vous. Plus tard dans ce laboratoire, vous attacherez ce rôle à une fonction Lambda que vous créerez. Ce rôle définit les permissions que la fonction Lambda aura lorsqu'elle sera exécutée. La politique permettra à la fonction Lambda d'ajouter ou de supprimer des règles entrantes sur les groupes de sécurité Amazon EC2. La politique permettra également à la fonction Lambda de créer et d'écrire des événements dans les logs CloudWatch.

Un deuxième rôle IAM personnalisé a également été créé pour vous dans le compte. Vous examinerez ce rôle et le modifierez dans les étapes suivantes.

2. Mettez à jour les permissions accordées au rôle IAM **AwsConfigRole**.
   - Dans le panneau de navigation, choisissez **Roles**.
   - Choisissez le lien **AwsConfigRole**.
   - Dans l'onglet **Permissions**, développez la politique **S3Access**, qui est déjà attachée à ce rôle.

Actuellement, ce rôle accorde des permissions pour obtenir les listes de contrôle d'accès (ACL) des buckets Amazon Simple Storage Service (Amazon S3) et télécharger des objets dans un bucket S3 si certaines conditions sont remplies. Ces permissions permettront à AWS Config d'écrire des fichiers de logs CloudWatch dans Amazon S3.

Près du haut de l'onglet, choisissez **Add permissions > Attach policies**.

Pour rechercher des politiques liées à AWS Config, dans la boîte de recherche, recherchez **Config** et appuyez sur Entrée.

Sélectionnez la politique **AWS_ConfigRole**.

Choisissez **Add permissions**, située en bas à droite.

Optionnellement, développez **AWS_ConfigRole** pour observer les détails de la politique.

La politique accorde un accès en lecture (principalement des actions Get, List et Describe) à de nombreux services AWS.

**Analyse :** Vous accorderez à AWS Config la possibilité d'utiliser ce rôle lorsque vous configurerez AWS Config dans la prochaine tâche. Le rôle définit les permissions qu'AWS Config aura lors de la surveillance d'une des régions dans le compte AWS.

### Tâche 2 : Configurer AWS Config pour surveiller les ressources
Dans cette tâche, vous configurerez AWS Config pour surveiller des ressources spécifiques dans une région du compte AWS.

1. Configurez AWS Config.
   - Dans la boîte de recherche à droite de **Services**, recherchez et choisissez **Config**.
   - Choisissez **Get started**, et configurez les paramètres suivants :
     - Sous **Recording strategy**, choisissez **Specific resource types**.
     - Type de ressource : choisissez **AWS EC2 SecurityGroup**. Pour la fréquence, choisissez **Continuous**.
     - Rôle IAM pour AWS Config : choisissez **Choose a role from your account**.
     - Rôles existants : choisissez **AwsConfigRole**.

**Remarque :** Rappelez-vous que **AwsConfigRole** était le deuxième rôle que vous avez analysé dans la tâche précédente.

Dans la section **Delivery method**, notez qu'AWS Config stockera les résultats dans un bucket S3 par défaut. Conservez les paramètres par défaut, puis choisissez **Next**.

Sur la page **AWS Managed Rules**, choisissez **Next** en bas de la page.

Passez en revue les détails de configuration d'AWS Config, puis choisissez **Confirm**.

Une bannière apparaît brièvement, puis le tableau de bord AWS Config s'affiche.

Pour observer l'inventaire des ressources créé par AWS Config, dans le panneau de navigation, choisissez **Resources**.

La page **Resource Inventory** s'affiche et liste les ressources Amazon EC2 de votre compte.

**Remarque :** Si la liste des ressources affiche un message indiquant que vos ressources sont en cours de découverte, attendez quelques minutes. Il peut falloir quelques minutes à AWS Config

 pour identifier toutes vos ressources.

**Analyse :** Rappelez-vous que vous avez configuré AWS Config pour inventorier les ressources de type **EC2 Security Group**. Les groupes de sécurité Amazon EC2 pré-approvisionnés dans la région où vous avez configuré AWS Config sont inclus dans l'inventaire. Cependant, notez que de nombreux autres types de ressources apparaissent également dans l'inventaire. AWS Config suit les ressources liées aux ressources qui vous intéressent principalement, car les ressources liées peuvent affecter le comportement des ressources principales. L'environnement de laboratoire dans lequel vous travaillez inclut de nombreuses ressources liées (telles que des passerelles Internet et des ACL réseau).

### Tâche 3 : Modifier un groupe de sécurité surveillé par AWS Config
Dans cette tâche, vous configurerez de nouveaux paramètres de règles entrantes dans l'un des groupes de sécurité répertoriés dans l'inventaire des ressources AWS Config. Le but est d'émuler un incident de sécurité. Certains des paramètres de règles entrantes que vous définirez pendant cette tâche ne correspondront pas aux paramètres souhaités, que vous définirez dans une tâche ultérieure.

1. Localisez le groupe de sécurité dans le Lab VPC.
   - Dans la boîte de recherche à droite de **Services**, recherchez et choisissez **VPC**.
   - Dans le panneau de navigation, choisissez la boîte **Filter by VPC**, puis choisissez **Lab VPC**.
   - Dans le panneau de navigation, choisissez **Security Groups**.

Au moins deux groupes de sécurité sont définis dans ce VPC.

Sélectionnez le groupe de sécurité **LabSG1**.

2. Ajoutez des règles entrantes au groupe de sécurité pour autoriser le trafic réseau HTTP, HTTPS, SMTPS et IMAPS.
   - Choisissez l'onglet **Inbound rules**, puis choisissez **Edit inbound rules**.
   - Notez qu'une règle entrante pour les connexions HTTP est déjà définie.
   - Pour la règle existante, changez **Source** en **Anywhere-IPv4**.
   - Choisissez **Add rule** et configurez comme suit :
     - Type : choisissez **HTTPS**.
     - Source : choisissez **Anywhere-IPv4**.
   - Choisissez **Add rule** à nouveau et configurez comme suit :
     - Type : choisissez **SMTPS**.
     - Source : choisissez **Anywhere-IPv4**.
   - Choisissez **Add rule** à nouveau et configurez comme suit :
     - Type : choisissez **IMAPS**.
     - Source : choisissez **Anywhere-IPv4**.
   - Choisissez **Save rules**.

Les règles entrantes devraient maintenant ressembler à celles de la capture d'écran suivante (bien que les ID de vos règles de groupe de sécurité soient différents).

### Tâche 4 : Créer une règle AWS Config qui appelle une fonction Lambda
Dans cette tâche, vous configurerez une règle AWS Config pour invoquer une fonction Lambda pré-créée. La règle et la fonction travailleront ensemble pour s'assurer que les groupes de sécurité Amazon EC2 surveillés n'ont que les règles entrantes souhaitées.

1. Allez dans la section **AWS Details** et copiez la valeur de **LambdaFunctionARN** dans votre presse-papiers.
   **Remarque :** Vous utiliserez l'ARN dans les étapes suivantes.

2. Créez une nouvelle règle AWS Config qui invoquera la fonction Lambda chaque fois que des groupes de sécurité Amazon EC2 surveillés sont modifiés.
   - Accédez à la console AWS Config.
   - Dans le panneau de navigation, choisissez **Rules**.
   - Actuellement, AWS Config n'a aucune règle définie.
   - Choisissez **Add rule**.
   - Pour **Select rule type**, choisissez **Create custom Lambda rule**.
   - Choisissez **Next**.
   - Sur la page **Configure rule**, configurez comme suit :
     - ARN de la fonction AWS Lambda : collez l'ARN de la fonction Lambda que vous avez copié.
     - Nom : entrez **EC2SecurityGroup**.
     - Description : entrez **Restrict inbound ports to HTTP and HTTPS**.
     - Type de déclencheur : sélectionnez **When configuration changes**.
     - Portée des modifications : choisissez **Resources**.
     - Type de ressource : choisissez **AWS EC2 SecurityGroup**.
   - AWS EC2 SecurityGroup apparaît dans la zone des ressources.
   - Dans la section **Parameters**, ajoutez un paramètre avec les paramètres suivants :
     - Clé : **debug**.
     - Valeur : **true**.

**Remarque :** Tous les paramètres que vous définissez ici seront passés par cette règle AWS Config à la fonction Lambda **EC2SecurityGroup**.

Choisissez **Next**, puis **Save**.

3. Observez les détails de la règle AWS Config **EC2SecurityGroup**.
   - Choisissez le lien **EC2SecurityGroup**.
   - Dans la section **Resources in scope**, choisissez le menu déroulant **Noncompliant**, puis choisissez **All**.
   - Dans la section **Rule details**, notez le champ **Last successful evaluation**. Initialement, ce champ affiche **Not available**; cependant, après quelques minutes, un horodatage s'affichera.

**Remarque :** L'évaluation initiale peut prendre quelques minutes. Cette même évaluation se produira également lorsque tout groupe de sécurité concerné sera modifié à l'avenir.

Notez les ressources de groupe de sécurité Amazon EC2 répertoriées comme étant dans le périmètre.

Pendant que l'évaluation initiale se déroule, la valeur de conformité sera **No results available**. Après plusieurs minutes, la valeur de chaque ressource de groupe de sécurité change en **Compliant**. Attendez de voir qu'il est conforme.

Notez que la colonne **Annotation** affiche **Permissions were modified**.

### Tâche 5 : Réexaminer la configuration du groupe de sécurité
Maintenant que l'évaluation de conformité initiale d'AWS Config a eu lieu, vous réexaminerez le groupe de sécurité **LabSG1**. Vous observerez si les modifications des règles entrantes (l'incident de sécurité que vous avez créé) ont été remarquées puis remédiées.

1. Analysez les règles entrantes définies sur le groupe de sécurité **LabSG1**.
   - Accédez à la console VPC.
   - Dans le panneau de navigation, choisissez la boîte **Filter by VPC**, puis choisissez **Lab VPC**.
   - Dans le panneau de navigation, choisissez **Security Groups**.
   - Sélectionnez le groupe de sécurité **LabSG1**.
   - Dans l'onglet **Inbound rules**, notez que seul le trafic HTTP et HTTPS est autorisé.

Les règles entrantes devraient maintenant ressembler à celles de la capture d'écran suivante (bien que les ID de vos règles de groupe de sécurité soient différents).

**Analyse :** Rappelez-vous que vous avez défini des règles entrantes pour **SMTPS** et **IMAPS**, ainsi que **HTTP** et **HTTPS**, sur ce groupe de sécurité. Cependant, les règles pour **SMTPS** et **IMAPS** n'existent plus. Rappelez-vous également que vous avez défini la version IP pour toutes les règles sur **IPv4 seulement**, mais maintenant les règles **HTTP** et **HTTPS** sont définies pour **IPv4** et **IPv6**.

En résumé, vous avez modifié les règles entrantes dans ce groupe de sécurité pour qu'elles ressemblent à celles de la capture d'écran suivante. Cependant, elles ont été significativement modifiées pour ressembler à la capture d'écran précédente.

### Tâche 6 : Utiliser les logs CloudWatch pour la vérification
Dans cette tâche, vous analyserez les logs CloudWatch et filtrerez les entrées de logs pour trouver des preuves de la remédiation.

1. Localisez les logs montrant les preuves des modifications que la règle AWS Config et la fonction Lambda associée ont apportées au groupe de sécurité.
   - Dans la boîte de recherche à droite de **Services**, recherchez et choisissez **CloudWatch**.
   - Dans le panneau de navigation, développez **Logs** puis choisissez **Log groups**.
   - Choisissez le lien du groupe de logs **awsconfig_lambda_security_group**.
   - Une ou plusieurs entrées de flux de logs sont visibles dans la liste des flux de logs.
   - Choisissez **Search all**.
   - Dans le champ de recherche **Filter events**, entrez **revoking for** puis appuyez sur Entrée.
   - Développez chaque événement de log et passez en revue le contenu.

Chaque événement fournit des détails sur l'action que la fonction Lambda a effectuée. Dans l'un des événements, vous devriez trouver des détails montrant que les règles entrantes que vous avez ajoutées manuellement pour **SMTPS** (port TCP 465) et **IMAPS** (port TCP 993) ont été supprimées.

Les autres événements filtrés ont enregistré les modifications apportées aux deux autres groupes de sécurité existant dans votre compte. Ces groupes de sécurité sont également inclus dans l'inventaire des ressources que votre règle AWS Config surveille.

**Analyse :** Vous avez observé des preuves dans les logs CloudWatch qu'AWS Config a invoqué la fonction Lambda pour révoquer automatiquement les modifications apportées au groupe de sécurité.

### Soumettre votre travail
Pour enregistrer votre progression, choisissez **Submit** en haut de ces instructions.

Lorsque vous y êtes invité, choisissez **Yes**.

Après quelques minutes, le panneau de notes apparaît et montre combien de points vous avez gagnés pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez **Grades** en haut de ces

 instructions.

**Conseil :** Vous pouvez soumettre votre travail plusieurs fois. Après avoir modifié votre travail, choisissez **Submit** à nouveau. Votre dernière soumission est enregistrée pour ce laboratoire.

Pour trouver des commentaires détaillés sur votre travail, choisissez **Submission Report**.

### Lab terminé
Félicitations ! Vous avez terminé le laboratoire.

En haut de cette page, choisissez **End Lab**, puis choisissez **Yes** pour confirmer que vous voulez terminer le laboratoire.

Un panneau de message indique que le laboratoire est en cours de terminaison.

Pour fermer le panneau, choisissez **Close** en haut à droite.

