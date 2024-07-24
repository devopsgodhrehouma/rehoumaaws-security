# Partie 1 - Tutoriel Complet sur les Différences entre AWS EventBridge et AWS Lambda

AWS EventBridge et AWS Lambda sont deux services essentiels dans l'écosystème AWS, chacun ayant des rôles distincts mais souvent complémentaires. Voici un guide détaillé sur ces services, leurs différences et comment ils travaillent ensemble dans divers scénarios AWS.

#### AWS EventBridge

**AWS EventBridge** est un service de bus d'événements sans serveur qui permet de connecter des applications en utilisant des données provenant de vos propres applications, d'applications SaaS intégrées et de services AWS. EventBridge fournit un flux de données en temps réel à partir de sources d'événements et dirige ces données vers des cibles telles que AWS Lambda, EC2, SQS, SNS, et plus encore.

**Caractéristiques Clés :**
1. **Architecture basée sur les événements :** Répond automatiquement aux changements dans vos applications.
2. **Routage d'événements :** Dirige les événements vers une variété de cibles en fonction des règles que vous définissez.
3. **Intégration :** S'intègre à plus de 100 services AWS et à de nombreuses applications SaaS.
4. **Registre de schémas :** Stocke la structure des événements et aide à générer des liaisons de code.

**Cas d'Utilisation Courants :**
- **Découplage des applications :** Garantit que les changements dans un service n'impactent pas directement les autres.
- **Traitement des événements :** Traite des flux de données en temps réel, déclenchant des actions basées sur des conditions spécifiques.
- **Surveillance et audit :** Envoie des événements vers des services de surveillance et d'audit tels que CloudWatch et AWS Config.

#### AWS Lambda

**AWS Lambda** est un service de calcul sans serveur qui vous permet d'exécuter du code sans avoir à provisionner ou à gérer des serveurs. Vous pouvez déclencher des fonctions Lambda à partir de plus de 200 services AWS et applications SaaS.

**Caractéristiques Clés :**
1. **Calcul à la demande :** S'adapte automatiquement en fonction du nombre de requêtes entrantes.
2. **Sécurité intégrée :** S'intègre facilement avec AWS Identity and Access Management (IAM) pour des opérations sécurisées.
3. **Paiement à l'utilisation :** Facture uniquement le temps de calcul consommé.

**Cas d'Utilisation Courants :**
- **Automatisation des tâches :** Exécute des scripts pour automatiser des tâches récurrentes.
- **Traitement en temps réel :** Traite des données en temps réel à partir de diverses sources d'événements.
- **Gestion des infrastructures :** Automatise les tâches d'infrastructure telles que la gestion des groupes de sécurité EC2.

#### Comparaison et Intégration

**Différences Principales :**
- **Fonction :** EventBridge est un service de routage d'événements tandis que Lambda est un service de calcul.
- **Utilisation :** EventBridge est utilisé pour transmettre des événements entre différentes applications et services, tandis que Lambda exécute du code en réponse à ces événements.
- **Cas d'utilisation :** EventBridge est idéal pour le découplage des systèmes et le routage des événements, tandis que Lambda est utilisé pour le traitement des données et l'exécution de tâches spécifiques.

**Exemple de Lab: AWS Config + Lambda pour Gérer les Groupes de Sécurité EC2 :**
1. **AWS Config :** Surveille les configurations des ressources AWS et génère des événements lorsque des changements sont détectés.
2. **AWS Lambda :** Traite ces événements pour appliquer des règles de conformité ou effectuer des corrections automatiques.
3. **AWS EventBridge :** Peut être utilisé pour capturer et router ces événements vers des cibles spécifiques pour un traitement supplémentaire ou une notification.

**Exemple de Lab: CloudTrail, CloudWatch et EventBridge :**
1. **AWS CloudTrail :** Suit les appels d'API et enregistre les événements.
2. **AWS CloudWatch :** Surveille les ressources AWS et peut déclencher des alarmes.
3. **AWS EventBridge :** Utilisé pour router les événements CloudTrail vers des cibles Lambda pour un traitement automatisé, comme la génération d'alertes ou la mise à jour des tableaux de bord.

En combinant ces services, vous pouvez créer des architectures robustes et scalables qui répondent de manière dynamique aux événements en temps réel, automatisant ainsi de nombreuses tâches de gestion et de surveillance de votre infrastructure AWS.

# Partie 2 (Débutants) - Tutoriel Complet sur les Différences entre AWS EventBridge et AWS Lambda pour Débutants

Pour bien comprendre AWS EventBridge et AWS Lambda, imaginons d'abord quelques analogies de la vie réelle et expliquons les concepts de base.

#### Qu'est-ce qu'AWS EventBridge ?

**AWS EventBridge** peut être comparé à un **système postal intelligent**. Imaginez que vous avez un réseau de boîtes aux lettres spéciales dans une ville. Chacune de ces boîtes est configurée pour recevoir uniquement des types spécifiques de courriers (ou événements) et les acheminer vers différentes destinations en fonction de leur contenu.

- **Routage des événements (courriers)** : Lorsqu'un événement survient (par exemple, une nouvelle commande en ligne), il est capté par EventBridge, qui décide où envoyer cet événement en fonction de règles prédéfinies.
- **Intégration avec des services** : EventBridge peut envoyer des événements à divers services AWS (comme Lambda, SQS, SNS) ou à des applications SaaS (Software as a Service), comme Salesforce ou Zendesk.
- **Automatisation** : Vous pouvez automatiser des actions en fonction des événements reçus, comme envoyer une notification, mettre à jour une base de données, ou déclencher un processus de traitement.

#### Qu'est-ce qu'AWS Lambda ?

**AWS Lambda** est comme un **robot multifonctionnel programmable**. Imaginez que ce robot est dans une pièce et attend des instructions spécifiques pour accomplir des tâches. Il ne fait rien jusqu'à ce qu'il reçoive une tâche (ou un événement) à accomplir.

- **Exécution de code sur demande** : Lambda exécute votre code uniquement lorsque des événements surviennent. Par exemple, lorsque vous recevez un nouvel email, Lambda peut être configuré pour analyser cet email et stocker des informations dans une base de données.
- **Pas de gestion de serveurs** : Vous n'avez pas à vous soucier des serveurs sur lesquels le code s'exécute. AWS s'en occupe pour vous.
- **Tarification basée sur l'utilisation** : Vous ne payez que pour le temps de calcul que votre code utilise, ce qui peut être très économique.

#### Comparaison des Rôles d'EventBridge et de Lambda

1. **Routage vs Exécution**
   - **EventBridge** : C'est le système postal intelligent qui décide où envoyer chaque courrier (événement).
   - **Lambda** : C'est le robot qui exécute des tâches spécifiques en réponse aux courriers (événements) qu'il reçoit.

2. **Fonctionnalités**
   - **EventBridge** : Routage des événements, création de règles pour filtrer et diriger les événements, intégration avec de nombreux services et applications.
   - **Lambda** : Exécution de fonctions (code) en réponse à des événements, sans gestion de serveurs, et échelle automatique selon les besoins.

3. **Cas d'utilisation en combinaison**
   - **Exemple de Lab 1: AWS Config + Lambda pour Gérer les Groupes de Sécurité EC2**
     - **AWS Config** surveille les configurations des ressources AWS.
     - **EventBridge** capture les événements de changement de configuration et les envoie à **Lambda**.
     - **Lambda** exécute du code pour vérifier et ajuster les groupes de sécurité EC2 en fonction des règles de conformité.

   - **Exemple de Lab 2: CloudTrail, CloudWatch et EventBridge**
     - **CloudTrail** enregistre les appels d'API et les événements d'activité.
     - **CloudWatch** surveille les métriques et peut déclencher des alarmes.
     - **EventBridge** capture les événements de CloudTrail et les dirige vers **Lambda** pour un traitement automatisé (par exemple, alerte de sécurité, mise à jour de tableau de bord).

#### Vulgarisation et Exemple de la Vie Réelle

Imaginez une boulangerie qui utilise AWS pour gérer ses opérations :

1. **AWS EventBridge : Le Chef de la Boulangerie**
   - Il surveille tout ce qui se passe dans la boulangerie.
   - Lorsque quelqu'un passe une commande, le chef décide qui doit être informé et quelle action doit être prise (par exemple, informer le pâtissier de préparer un gâteau).

2. **AWS Lambda : Les Employés Polyvalents**
   - Ils attendent les instructions du chef.
   - Une fois informés, ils effectuent la tâche spécifique : préparer le gâteau, envoyer une confirmation au client, ou mettre à jour le stock.

En résumé, **EventBridge** s'assure que les bons événements sont envoyés aux bonnes personnes, tandis que **Lambda** exécute les actions nécessaires en réponse à ces événements. Ensemble, ils permettent une automatisation et une gestion efficaces des tâches, tout en réduisant la nécessité de gestion manuelle et d'infrastructure complexe.
