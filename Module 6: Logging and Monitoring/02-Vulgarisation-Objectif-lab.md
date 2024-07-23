# Vulgarisation et Explication de l'Objectif Global du Laboratoire
### Vulgarisation et Explication de l'Objectif Global du Laboratoire

**Objectif Global :**
Ce laboratoire vise à vous apprendre à surveiller et à recevoir des alertes sur les activités qui se produisent dans un compte AWS. En utilisant des services comme CloudTrail, CloudWatch, SNS et EventBridge, vous pourrez configurer des mécanismes qui vous permettent de détecter des événements spécifiques et d'être alerté rapidement pour prendre les actions nécessaires.

**Pourquoi c'est important :**
Dans un environnement cloud, il est essentiel de surveiller ce qui se passe pour garantir la sécurité et la conformité. Par exemple, savoir quand quelqu'un modifie des règles de sécurité ou tente de se connecter de manière incorrecte à plusieurs reprises peut vous aider à identifier et à réagir aux menaces potentielles.

### Étapes Clés du Laboratoire :

1. **Surveillance des Appels d'API avec CloudTrail :**
   - **CloudTrail** : Ce service vous permet de suivre les appels d'API dans votre compte AWS. Un appel d'API est une action que vous ou un service AWS pouvez effectuer, comme créer une machine virtuelle ou modifier une règle de sécurité.
   - **Historique des Événements** : Vous allez apprendre à consulter cet historique pour voir les actions qui ont été effectuées récemment.

2. **Création d'Alerte Email avec SNS :**
   - **SNS (Simple Notification Service)** : Il permet d'envoyer des notifications par email ou SMS. Vous allez créer un sujet SNS et vous abonner avec votre email pour recevoir des notifications.
   - **Utilisation** : Lorsqu'un événement important se produit, une alerte vous sera envoyée par email.

3. **Surveillance des Modifications de Sécurité avec EventBridge :**
   - **EventBridge** : C'est un service qui permet de réagir à des événements spécifiques. Par exemple, si quelqu'un modifie un groupe de sécurité, vous pouvez configurer une règle pour être alerté.
   - **Configuration** : Vous allez créer une règle qui surveille les modifications des groupes de sécurité et envoie une alerte via SNS.

4. **Création d'Alarme de Connexion Échouée avec CloudWatch :**
   - **CloudWatch** : Utilisé pour surveiller les métriques et créer des alarmes. Par exemple, vous pouvez créer une alarme qui vous alerte si quelqu'un tente de se connecter plusieurs fois sans succès.
   - **Métrique et Filtre** : Vous allez définir un filtre de métriques pour surveiller les tentatives de connexion échouées et configurer une alarme basée sur cette métrique.

5. **Analyse des Journaux avec CloudWatch Logs Insights :**
   - **CloudWatch Logs Insights** : C'est un outil pour analyser les journaux. Vous pouvez exécuter des requêtes pour extraire des informations spécifiques, comme les échecs de connexion.
   - **Requête** : Vous allez apprendre à écrire et exécuter des requêtes pour analyser les journaux CloudTrail.

### Conclusion :
En suivant ce laboratoire, vous comprendrez comment mettre en place une surveillance efficace et recevoir des alertes sur les activités cruciales dans votre compte AWS. Cela vous aidera à réagir rapidement aux incidents de sécurité et à maintenir la conformité dans votre environnement cloud.
