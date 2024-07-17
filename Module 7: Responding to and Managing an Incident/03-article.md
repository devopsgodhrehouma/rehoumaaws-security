## Informations dont vous avez besoin sur AWS Config
# https://medium.com/@raggadaz/aws-config-ef084306cdcb
### Qu'est-ce qu'AWS Config ?

AWS Config est un service entièrement géré qui offre les fonctionnalités suivantes pour améliorer la sécurité et la gouvernance de votre environnement AWS :
- **Inventaire des ressources AWS** : Crée une liste détaillée de toutes vos ressources AWS.
- **Historique de configuration** : Suivi des modifications apportées aux configurations de vos ressources.
- **Notifications de changement de configuration** : Envoie des alertes en cas de modification de la configuration.

AWS Config permet d'effectuer un audit complet et de visualiser l'état de votre environnement AWS à tout moment et dans n'importe quelle région du monde. Il facilite l'audit de conformité, l'analyse de sécurité et le suivi des ressources.

### Fonctionnalités principales

- **Instantanés de configuration** : Fournit des instantanés des configurations et enregistre les modifications apportées aux ressources AWS.
- **Contrôle de conformité automatisé** : Permet de vérifier automatiquement la conformité des configurations.

### Composants principaux

Les composants principaux d'AWS Config sont :
- **Tableau de bord de configuration**
- **Règles de configuration**
- **Ressources**
- **Paramètres**

Les règles de configuration peuvent être des règles AWS standard ou des règles personnalisées définies par vous. Au moment de la rédaction de cet article, il existe 86 règles AWS standard.

### Fonctionnement

AWS Config est un service basé sur la région, ce qui signifie que vous devez l'activer et le configurer pour chaque région utilisée.

AWS Config fonctionne de manière similaire à CloudTrail :
1. Lorsqu'une modification est apportée, par exemple à un groupe de sécurité, un événement est créé.
2. AWS Config récupère cet événement.
3. AWS Config stocke toutes les informations dans le compartiment S3 configuré.
4. La suite des opérations dépend de la configuration, par exemple, l'utilisation de règles standard AWS ou de règles personnalisées.

Vous pouvez, par exemple, programmer une fonction Lambda pour rechercher constamment de nouveaux événements dans la configuration AWS ou vérifier avec Lambda s'il existe une règle non respectée et déclencher une notification SNS.

### Terminologie d'AWS Config

- **Éléments de configuration** : Attributs ponctuels d'une ressource.
- **Instantanés de configuration** : Collection d'éléments de configuration.
- **Flux de configuration** : Flux d'éléments de configuration modifiés.
- **Historique de configuration** : Collection d'éléments de configuration pour une ressource au fil du temps.
- **Enregistreur de configuration** : La configuration enregistre et stocke les éléments de configuration.

### Informations visibles dans AWS Config

- **Type de ressource**
- **ID de la ressource**
- **Conformité**
- **Chronologie**

Dans la chronologie, vous pouvez voir les détails de configuration, les relations de vos ressources, quand la ressource a été créée, tous les changements apportés et les événements CloudTrail.

### Autorisations requises

Le rôle IAM que vous devez créer pour AWS Config nécessite les autorisations suivantes :
- **Autorisations en lecture seule sur les ressources enregistrées**
- **Accès en écriture au bucket de journalisation S3**
- **Accès en publication aux services de notifications courtes (SNS)**

### Exemple de configuration AWS en mots et en images

1. Cliquez sur **Commencer**.
2. Incluez les ressources mondiales comme IAM.
3. Créez un bucket S3.
4. Activez les notifications SNS uniquement si vous souhaitez être averti de chaque changement de configuration.
5. Créez un nouveau rôle pour Config dans IAM (ou choisissez-en un existant).

Après cela, vous pouvez ajouter les règles de configuration (il y en a 86 prédéfinies pour le moment).

Dans cet exemple, nous voulons ajouter une règle avec **ssh**.

Après cela, cliquez sur **suivant** et **confirmer** et votre première configuration est terminée.

Dans cette règle SSH, nous avons décidé de vérifier : **Si les groupes de sécurité interdisent le trafic SSH entrant sans restriction**. Dans mon compte, j'ai créé un groupe de sécurité avec SSH autorisé à partir de 0.0.0.0/0, ce qui conduit à une règle non respectée comme vous pouvez le voir dans les images suivantes.

#### Exemple visuel

- **Le groupe de sécurité qui conduit à une règle non respectée**
- **Le tableau de bord montrant une règle non respectée**
- **Le groupe de sécurité non conforme**

Après avoir supprimé l'entrée SSH dans le groupe de sécurité, revenez à AWS Config, choisissez la règle non respectée et cliquez sur **Réévaluer**. Après cela, tout sera conforme.

J'espère que cet exemple d'utilisation d'AWS Config était intéressant. Bonne lecture et continuez à être génial !
