# Cours AWS : Sécurité et Gestion dans le Cloud

## Table des Matières

1. [Module 2 : Concepts de base du cloud](#module-2-concepts-de-base-du-cloud)
    1. [Quel est l'avantage de l'élasticité dans le cloud ?](#quel-est-lavantage-de-lélasticité-dans-le-cloud)
    2. [Quel est le modèle de tarification que les clients AWS peuvent utiliser pour payer les ressources selon leurs besoins ?](#quel-est-le-modèle-de-tarification-que-les-clients-aws-peuvent-utiliser-pour-payer-les-ressources-selon-leurs-besoins)
    3. [Quelle est une méthode qu'une entreprise pourrait utiliser pour garantir une haute disponibilité lors d'une attaque de sécurité ?](#quelle-est-une-méthode-quune-entreprise-pourrait-utiliser-pour-garantir-une-haute-disponibilité-lors-dune-attaque-de-sécurité)
    4. [Quel principe de sécurité traite de la surveillance, de l'alerte et de l'audit des actions et des changements de l'environnement en temps réel ?](#quel-principe-de-sécurité-traite-de-la-surveillance-de-lalerte-et-de-laudit-des-actions-et-des-changements-de-lenvironnement-en-temps-réel)
    5. [Quelle est une meilleure pratique pour l'automatisation qui peut aider à fournir une infrastructure sécurisée fiable et répétable ?](#quelle-est-une-meilleure-pratique-pour-lautomatisation-qui-peut-aider-à-fournir-une-infrastructure-sécurisée-fiable-et-répétable)
    6. [Quelles options sont des caractéristiques du principe du moindre privilège ? (Sélectionnez DEUX.)](#quelles-options-sont-des-caractéristiques-du-principe-du-moindre-privilège-sélectionnez-deux)
    7. [Quelles options sont des principes de sécurité basés sur le pilier de sécurité du cadre AWS bien architecturé ? (Sélectionnez TROIS.)](#quelles-options-sont-des-principes-de-sécurité-basés-sur-le-pilier-de-sécurité-du-cadre-aws-bien-architecturé-sélectionnez-trois)
    8. [La sécurité et la conformité sont des responsabilités partagées entre AWS et le client. Quelle est une responsabilité AWS ?](#la-sécurité-et-la-conformité-sont-des-responsabilités-partagées-entre-aws-et-le-client-quelle-est-une-responsabilité-aws)
    9. [Quelles sont les responsabilités de sécurité des clients selon le modèle de responsabilité partagée AWS ? (Sélectionnez DEUX.)](#quelles-sont-les-responsabilités-de-sécurité-des-clients-selon-le-modèle-de-responsabilité-partagée-aws-sélectionnez-deux)
    10. [Qui est responsable de l'exploitation, de la gestion et du contrôle de la sécurité DE l'infrastructure cloud, selon le modèle de responsabilité partagée AWS ?](#qui-est-responsable-de-lexploitation-de-la-gestion-et-du-contrôle-de-la-sécurité-de-linfrastructure-cloud-selon-le-modèle-de-responsabilité-partagée-aws)
2. [Module 3 : Gestion des identités et des accès (IAM)](#module-3-gestion-des-identités-et-des-accès-iam)
    1. [Quelle affirmation concernant la gestion des identités et des accès (IAM) d'AWS est vraie ?](#quelle-affirmation-concernant-la-gestion-des-identités-et-des-accès-iam-daws-est-vraie)
    2. [Quelle option est considérée comme une meilleure pratique pour configurer l'accès à long terme dans la gestion des identités et des accès (IAM) d'AWS ?](#quelle-option-est-considérée-comme-une-meilleure-pratique-pour-configurer-laccès-à-long-terme-dans-la-gestion-des-identités-et-des-accès-iam-daws)
    3. [Quelle affirmation décrit le mieux un rôle de la gestion des identités et des accès (IAM) d'AWS ?](#quelle-affirmation-décrit-le-mieux-un-rôle-de-la-gestion-des-identités-et-des-accès-iam-daws)
    4. [Quelle méthode permettrait de réaliser une authentification multi-facteurs (MFA) ?](#quelle-méthode-permettrait-de-réaliser-une-authentification-multi-facteurs-mfa)
    5. [Quelle affirmation concernant les politiques de gestion des identités et des accès (IAM) d'AWS est vraie ?](#quelle-affirmation-concernant-les-politiques-de-gestion-des-identités-et-des-accès-iam-daws-est-vraie)
    6. [Un administrateur a décidé d'utiliser des politiques en ligne pour améliorer la posture de sécurité de son organisation. Quelle affirmation concernant les politiques en ligne est vraie ?](#un-administrateur-a-décidé-dutiliser-des-politiques-en-ligne-pour-améliorer-la-posture-de-sécurité-de-son-organisation-quelle-affirmation-concernant-les-politiques-en-ligne-est-vraie)
    7. [Quelle méthode fournirait la fédération des identités ?](#quelle-méthode-fournirait-la-fédération-des-identités)
    8. [Quel service AWS repose sur des pools d'utilisateurs et des pools d'identités ?](#quel-service-aws-repose-sur-des-pools-dutilisateurs-et-des-pools-didentités)
    9. [Quelle affirmation concernant AWS Organizations est vraie ?](#quelle-affirmation-concernant-aws-organizations-est-vraie)
    10. [Quel service AWS la gestion des identités et des accès (IAM) d'AWS utilise-t-elle pour fournir des informations d'identification de sécurité temporaires pour les rôles ?](#quel-service-aws-la-gestion-des-identités-et-des-accès-iam-daws-utilise-t-elle-pour-fournir-des-informations-didentification-de-sécurité-temporaire-pour-les-rôles)
3. [Module 5 : Sécurité et conformité](#module-5-sécurité-et-conformité)
    1. [Quels éléments peuvent être utilisés pour contrôler l'accès à un bucket Amazon S3 ? (Sélectionnez TROIS.)](#quels-éléments-peuvent-être-utilisés-pour-contrôler-laccès-à-un-bucket-amazon-s3-sélectionnez-trois)
    2. [Quel mécanisme de contrôle est recommandé pour protéger les données en transit vers Amazon S3 afin de réduire le risque d'accès non autorisé ou de perte ?](#quel-mécanisme-de-contrôle-est-recommandé-pour-protéger-les-données-en-transit-vers-amazon-s3-afin-de-réduire-le-risque-daccès-non-autorisé-ou-de-perte)
    3. [Amazon S3 Object Lock est une fonctionnalité qui offre une protection pour les scénarios où il est impératif que les données ne soient pas modifiées ou supprimées. Quelles sont les deux façons de gérer la rétention d'objets avec Object Lock ?](#amazon-s3-object-lock-est-une-fonctionnalité-qui-offre-une-protection-pour-les-scénarios-où-il-est-impératif-que-les-données-ne-soient-pas-modifiées-ou-supprimées-quelles-sont-les-deux-façons-de-gérer-la-rétention-dobjets-avec-object-lock)
    4. [Quelle fonctionnalité d'Amazon S3 stocke des objets en utilisant le modèle écrire-une-fois-lire-plusieurs (WORM) ?](#quelle-fonctionnalité-damazon-s3-stocke-des-objets-en-utilisant-le-modèle-écrire-une-fois-lire-plusieurs-worm)
    5. [Quel service AWS peut être utilisé pour générer des clés de cryptage protégées par des modules de sécurité matériels validés FIPS 140-2 (HSM) ?](#quel-service-aws-peut-être-utilisé-pour-générer-des-clés-de-cryptage-protégées-par-des-modules-de-sécurité-matériels-validés-fips-140-2-hsm)
    6. [Quels types de cryptage côté serveur (SSE) sont disponibles pour Amazon S3 ? (Sélectionnez TROIS.)](#quels-types-de-cryptage-côté-serveur-sse-sont-disponibles-pour-amazon-s3-sélectionnez-trois)
    7. [Quelle affirmation concernant le cryptage côté client est vraie ?](#quelle-affirmation-concernant-le-cryptage-côté-client-est-vraie)
    8. [Quelle affirmation est vraie à propos du cryptage côté serveur dans Amazon S3 avec des clés fournies par le client (SSE-C) ?](#quelle-affirmation-est-vraie-à-propos-du-cryptage-côté-serveur-dans-amazon-s3-avec-des-clés-fournies-par-le-client-sse-c)
    9. [Quel service AWS peut être utilisé pour stocker des éléments, tels que des informations d'identification de base de données, de manière sécurisée sans avoir à les coder en dur dans le code source ou un fichier de configuration ?

](#quel-service-aws-peut-être-utilisé-pour-stocker-des-éléments-tels-que-des-informations-didentification-de-base-de-données-de-manière-sécurisée-sans-avoir-à-les-coder-en-dur-dans-le-code-source-ou-un-fichier-de-configuration)
    10. [Un ingénieur en apprentissage automatique souhaite une solution qui découvrira les informations sensibles stockées dans AWS. La solution doit également utiliser le traitement du langage naturel (NLP) pour classifier les données et fournir des insights liés aux affaires. Quel service AWS répondrait à leurs besoins ?](#un-ingénieur-en-apprentissage-automatique-souhaite-une-solution-qui-découvrira-les-informations-sensibles-stockées-dans-aws-la-solution-doit-également-utiliser-le-traitement-du-langage-naturel-nlp-pour-classifier-les-données-et-fournir-des-insights-liés-aux-affaires-quel-service-aws-répondrait-à-leurs-besoins)
4. [Module 6 : Surveillance et journalisation](#module-6-surveillance-et-journalisation)
    1. [Quelle affirmation concernant les journaux et les fichiers journaux est vraie ?](#quelle-affirmation-concernant-les-journaux-et-les-fichiers-journaux-est-vraie)
    2. [Quelle affirmation concernant la surveillance AWS est vraie ?](#quelle-affirmation-concernant-la-surveillance-aws-est-vraie)
    3. [Quelle affirmation concernant AWS CloudTrail est vraie ?](#quelle-affirmation-concernant-aws-cloudtrail-est-vraie)
    4. [Quelle information AWS CloudTrail NE capture-t-elle PAS ?](#quelle-information-aws-cloudtrail-ne-capture-t-elle-pas)
    5. [Une application web utilise une flotte d'instances Amazon EC2 pour des actifs dynamiques et statiques. Les instances EC2 sont dans un sous-réseau privé, derrière un équilibreur de charge qui est dans un sous-réseau public à l'intérieur du VPC. Quels journaux de service fourniraient le PLUS d'informations sur la façon dont les utilisateurs utilisent l'application web ?](#une-application-web-utilise-une-flotte-dinstances-amazon-ec2-pour-des-actifs-dynamiques-et-statiques-les-instances-ec2-sont-dans-un-sous-réseau-privé-derrière-un-équilibreur-de-charge-qui-est-dans-un-sous-réseau-public-à-lintérieur-du-vpc-quels-journaux-de-service-fourniraient-le-plus-dinformations-sur-la-façon-dont-les-utilisateurs-utilisent-lapplication-web)
    6. [Quelle affirmation concernant Amazon CloudWatch est vraie ?](#quelle-affirmation-concernant-amazon-cloudwatch-est-vraie)
    7. [AWS CloudTrail et Amazon CloudWatch remplissent des fonctions spécifiques. Quelle fonction est indicative de CloudWatch ?](#aws-cloudtrail-et-amazon-cloudwatch-remplissent-des-fonctions-spécifiques-quelle-fonction-est-indicative-de-cloudwatch)
    8. [Quelle est la première étape pour utiliser les contrôles détectifs que AWS fournit ?](#quelle-est-la-première-étape-pour-utiliser-les-contrôles-détectifs-que-aws-fournit)
    9. [Quel service AWS évalue votre compte en utilisant des vérifications qui identifient des moyens d'optimiser votre infrastructure AWS, d'améliorer la sécurité et les performances, de réduire les coûts et de surveiller les quotas de service ?](#quel-service-aws-évalue-votre-compte-en-utilisant-des-vérifications-qui-identifient-des-moyens-doptimiser-votre-infrastructure-aws-daméliorer-la-sécurité-et-les-performances-de-réduire-les-coûts-et-de-surveiller-les-quotas-de-service)
    10. [Quel service de journalisation et de surveillance AWS est un service de bus d'événements sans serveur qui peut connecter vos applications avec des données provenant de diverses sources ?](#quel-service-de-journalisation-et-de-surveillance-aws-est-un-service-de-bus-dévéénements-sans-serveur-qui-peut-connecter-vos-applications-avec-des-données-provenant-de-diverses-sources)
5. [Module 7 : Sécurité et gestion dans AWS](#module-7-sécurité-et-gestion-dans-aws)
    1. [Quel service AWS est un service de surveillance et d'évaluation continue qui fournit un inventaire des ressources AWS et enregistre les modifications apportées à leur configuration ?](#quel-service-aws-est-un-service-de-surveillance-et-déévaluation-continue-qui-fournit-un-inventaire-des-ressources-aws-et-enregistre-les-modifications-apportées-à-leur-configuration)
    2. [Un administrateur souhaite obtenir de l'aide pour gérer des opérations complexes et chronophages, telles que la mise à jour des logiciels et le routage des tickets de support. Quel service AWS répondrait à leur besoin ?](#un-administrateur-souhaite-obtenir-de-laide-pour-gérer-des-opérations-complexes-et-chronophages-telles-que-la-mise-à-jour-des-logiciels-et-le-routage-des-tickets-de-support-quel-service-aws-répondrait-à-leur-besoin)
    3. [Un administrateur souhaite s'assurer qu'il optimise son infrastructure AWS et améliore également la performance de la sécurité. Quelle option répondrait à leur besoin ?](#un-administrateur-souhaite-sassurer-quil-optimise-son-infrastructure-aws-et-améliore-également-la-performance-de-la-sécurité-quelle-option-répondrait-à-leur-besoin)
    4. [Un administrateur souhaite avoir des informations supplémentaires sur leurs applications et collecter des données opérationnelles sous forme de journaux, de métriques et d'événements. Quel service AWS répondrait à leur besoin ?](#un-administrateur-souhaite-avoir-des-informations-supplémentaires-sur-leurs-applications-et-collecter-des-données-opérationnelles-sous-forme-de-journaux-de-métriques-et-déévénements-quel-service-aws-répondrait-à-leur-besoin)
    5. [Un administrateur souhaite améliorer la gestion de leurs vulnérabilités. Ils aimeraient utiliser un service qui scanne en continu les instances Amazon EC2 pour détecter les vulnérabilités logicielles et l'exposition non intentionnelle au réseau. Quel service AWS répondrait à leur besoin ?](#un-administrateur-souhaite-améliorer-la-gestion-de-leurs-vulnérabilités-ils-aimeraient-utiliser-un-service-qui-scanne-en-continu-les-instances-amazon-ec2-pour-détecter-les-vulnérabilités-logicielles-et-l'exposition-non-intentionnelle-au-réseau-quel-service-aws-répondrait-à-leur-besoin)
    6. [Un administrateur souhaite effectuer des vérifications d'évaluation continues sur leurs ressources. Le but est de vérifier que ces ressources respectent leurs propres politiques de sécurité, les meilleures pratiques de l'industrie et les normes de conformité. Quel service AWS répondrait à leur besoin ?](#un-administrateur-souhaite-effectuer-des-vérifications-déévaluation-continues-sur-leurs-ressources-le-but-est-de-vérifier-que-ces-ressources-respectent-leurs-propres-politiques-de-sécurité-les-meilleures-pratiques-de-lindustrie-et-les-normes-de-conformité-quel-service-aws-répondrait-à-leur-besoin)
    7. [Un administrateur souhaite augmenter la protection de leur réseau et éliminer toute activité potentiellement non autorisée ou malveillante dans leur environnement AWS. Quel service AWS répondrait à leur besoin ?](#un-administrateur-souhaite-augmenter-la-protection-de-leur-réseau-et-éliminer-toute-activité-potentiellement-non-autorisée-ou-malveillante-dans-leur-environnement-aws-quel-service-aws-répondrait-à-leur-besoin)
    8. [Quelles sont les meilleures pratiques pour gérer un incident ? (Sélectionnez DEUX.)](#quelles-sont-les-meilleures-pratiques-pour-gérer-un-incident-sélectionnez-deux)
    9. [Un administrateur souhaite mettre en œuvre un niveau de protection contre les attaques par déni de service distribué (DDoS). Quel service AWS répondrait à leur besoin ?](#un-administrateur-souhaite-mettre-en-œuvre-un-niveau-de-protection-contre-les-attaques-par-déni-de-service-distribué-ddos-quel-service-aws-répondrait-à-leur-besoin)
    10. [Quelle définition décrit le mieux une réponse aux incidents ?](#quelle-définition-décrit-le-mieux-une-réponse-aux-incidents)

---

## Module 2 : Concepts de base du cloud

### Quel est l'avantage de l'élasticité dans le cloud ?

L'élasticité est une caractéristique essentielle du cloud computing qui permet aux systèmes de s'adapter dynamiquement aux variations de la demande. Les avantages de l'élasticité dans le cloud incluent :

1. **Minimiser les exigences de stockage

 en réduisant les activités de journalisation et d'audit :** Ce n'est pas le principal avantage de l'élasticité. Cependant, l'élasticité peut indirectement contribuer à une utilisation plus efficace des ressources de stockage en ajustant les capacités en fonction de la demande réelle.
   
2. **Réduire le temps nécessaire pour rendre de nouvelles ressources informatiques disponibles aux développeurs, passant de semaines à minutes :** Grâce à l'élasticité, les ressources informatiques peuvent être provisionnées en quelques minutes, ce qui accélère considérablement le développement et le déploiement des applications.
   
3. **Créer des systèmes qui évoluent en fonction de la capacité requise selon les variations de la demande :** L'élasticité permet aux systèmes de s'ajuster automatiquement pour répondre aux variations de la demande, assurant ainsi une performance optimale et une disponibilité continue.

4. **Échanger des dépenses fixes contre des dépenses variables :** En utilisant l'élasticité, les entreprises peuvent réduire leurs coûts en payant uniquement pour les ressources qu'elles utilisent réellement, transformant ainsi des dépenses fixes en dépenses variables.

[Retour au sommet](#table-des-matières)

### Quel est le modèle de tarification que les clients AWS peuvent utiliser pour payer les ressources selon leurs besoins ?

AWS propose plusieurs modèles de tarification flexibles pour répondre aux différents besoins des clients. Le modèle de tarification le plus couramment utilisé est le **"payer à l'utilisation"** (Pay-As-You-Go). Voici un aperçu des options disponibles :

1. **Payer à la réservation :** Les clients peuvent réserver des instances pour une période déterminée (un ou trois ans) à un tarif réduit par rapport à l'utilisation à la demande.
   
2. **Payer à l'utilisation :** Ce modèle permet aux clients de payer uniquement pour les ressources qu'ils consomment, sans frais initiaux ni engagements à long terme. Cela offre une grande flexibilité et optimise les coûts en fonction de l'utilisation réelle.

3. **Payer à la désactivation :** Non applicable dans le contexte d'AWS, car les ressources sont facturées en fonction de leur utilisation active, même lorsqu'elles sont désactivées ou en veille.

4. **Payer à l'achat :** Non applicable, car AWS ne propose pas de modèle de tarification basé sur l'achat de ressources pour une utilisation continue.

[Retour au sommet](#table-des-matières)

### Quelle est une méthode qu'une entreprise pourrait utiliser pour garantir une haute disponibilité lors d'une attaque de sécurité ?

Pour garantir une haute disponibilité et assurer la résilience lors d'une attaque de sécurité, les entreprises peuvent utiliser plusieurs méthodes, dont la **mise à l'échelle automatique** (Auto Scaling). Voici les options possibles :

1. **Audits réguliers :** Bien que les audits réguliers soient importants pour identifier les vulnérabilités et les failles de sécurité, ils ne garantissent pas directement la haute disponibilité en cas d'attaque.
   
2. **Mise à l'échelle automatique :** Cette méthode permet d'ajuster automatiquement le nombre d'instances de calcul en fonction de la demande, assurant ainsi que les applications restent disponibles et performantes même en cas de forte demande ou d'attaque.

3. **Surveillance des ressources :** La surveillance des ressources est cruciale pour détecter les anomalies et les incidents, mais elle doit être complétée par des actions automatisées pour garantir la haute disponibilité.

4. **Contrôle d'accès :** Le contrôle d'accès aide à prévenir les accès non autorisés, mais il ne suffit pas à lui seul pour garantir la haute disponibilité lors d'une attaque.

[Retour au sommet](#table-des-matières)

### Quel principe de sécurité traite de la surveillance, de l'alerte et de l'audit des actions et des changements de l'environnement en temps réel ?

Le principe de sécurité qui traite de la surveillance, de l'alerte et de l'audit des actions et des changements de l'environnement en temps réel est **"Activer la traçabilité"**. Ce principe inclut les pratiques suivantes :

1. **Protéger les données en transit et au repos :** Ce principe se concentre sur la protection des données lorsqu'elles sont transférées ou stockées, mais ne traite pas spécifiquement de la surveillance en temps réel.

2. **Sécuriser toutes les couches :** Bien que ce principe soit crucial pour une approche de sécurité en profondeur, il ne se concentre pas spécifiquement sur la surveillance et l'audit.

3. **Appliquer le principe du moindre privilège :** Ce principe vise à limiter les permissions des utilisateurs et des systèmes à ce qui est strictement nécessaire, mais il ne traite pas de la surveillance et de l'audit.

4. **Activer la traçabilité :** Ce principe implique la surveillance continue des actions, l'alerte en cas d'anomalies et l'audit des changements dans l'environnement en temps réel pour assurer la sécurité et la conformité.

[Retour au sommet](#table-des-matières)

### Quelle est une meilleure pratique pour l'automatisation qui peut aider à fournir une infrastructure sécurisée fiable et répétable ?

Pour garantir une infrastructure sécurisée, fiable et répétable, il est recommandé de **"Implémenter l'infrastructure en tant que code"** (IaC). Voici un aperçu des meilleures pratiques :

1. **Utiliser des contrôles de détection :** Bien que les contrôles de détection soient essentiels pour identifier les incidents de sécurité, ils ne sont pas directement liés à l'automatisation de l'infrastructure.

2. **Utiliser le chiffrement et les contrôles d'accès :** Ces pratiques sont cruciales pour sécuriser les données, mais elles ne suffisent pas à elles seules pour garantir une infrastructure fiable et répétable.

3. **Implémenter l'infrastructure en tant que code :** Cette pratique consiste à utiliser des scripts et des modèles pour automatiser la configuration et la gestion de l'infrastructure, assurant ainsi la cohérence, la répétabilité et la rapidité des déploiements.

4. **Implémenter une solution de gestion des journaux :** La gestion des journaux est importante pour la surveillance et l'audit, mais elle ne garantit pas directement une infrastructure fiable et répétable.

[Retour au sommet](#table-des-matières)

### Quelles options sont des caractéristiques du principe du moindre privilège ? (Sélectionnez DEUX.)

Le **principe du moindre privilège** consiste à accorder aux utilisateurs et aux systèmes uniquement les permissions nécessaires pour accomplir leurs tâches. Voici les caractéristiques de ce principe :

1. **Utiliser différents services AWS :** Non directement lié au moindre privilège.

2. **Surveiller les actions et les changements :** Bien que la surveillance soit importante, elle n'est pas une caractéristique spécifique du moindre privilège.

3. **Faire respecter la séparation des tâches :** Cela implique de diviser les responsabilités entre différents utilisateurs pour réduire le risque de malveillance ou d'erreurs, ce qui est une caractéristique clé du moindre privilège.

4. **Utiliser le chiffrement et les contrôles d'accès :** Ces pratiques sont importantes pour la sécurité, mais ne sont pas spécifiques au moindre privilège.

5. **Accorder l'accès uniquement au besoin :** Cela signifie que les utilisateurs reçoivent uniquement les permissions nécessaires pour accomplir leurs tâches spécifiques, une caractéristique fondamentale du moindre privilège.

[Retour au sommet](#table-des-matières)

### Quelles options sont des principes de sécurité basés sur le pilier de sécurité du cadre AWS bien architecturé ? (Sélectionnez TROIS.)

Le **cadre AWS bien architecturé** repose sur plusieurs principes de sécurité. Les principes pertinents incluent :

1. **Utiliser la mise à l'échelle automatique :** Bien que cela soit important pour la performance et la disponibilité, ce n'est pas un principe de sécurité.

2. **Préparer les événements de sécurité :** Cela inclut l'anticipation des incidents de sécurité et la mise en place de plans de réponse appropriés, un principe essentiel de la sécurité.

3. **Démocratiser les technologies avancées :** Bien que cela soit bénéfique, ce n'est pas un principe de sécurité.

4. **Automatiser les meilleures pratiques de sécurité :** L'automatisation des pratiques de sécurité garantit que les contrôles sont appliqués de manière cohérente et rapide, réduisant ainsi le risque d'erreurs humaines.

5. **Concevoir vos systèmes pour une haute disponibilité :** Cela est crucial pour garantir que les systèmes restent disponibles même en cas d'incidents de sécurité.

6. **Protéger les données en transit et au repos :** Assurer la sécurité des données lorsqu'elles sont transférées ou stockées est un principe fondamental de la sécurité.

[Retour au sommet](#table-des-matières)

### La sécurité et la conformité sont des responsabilités partagées entre AWS et le client. Quelle est une responsabilité AWS ?

Dans le **modèle de responsabilité partagée** d'AWS, certaines responsabilités incombent à AWS, tandis que d'autres sont à la charge du client. AWS est responsable de :

1. **Configurer des groupes de sécurité :** C'est une responsabilité du client.
2. **Gérer les logiciels d'application :** C'est une responsabilité du client.
3. **Appliquer des correctifs à l'infrastructure réseau :** AWS est responsable de la maintenance et de la mise à jour de l'infrastructure réseau sous-jacente, y compris l'application des correctifs de sécurité.
4. **Gérer les données client :** C'est une responsabilité du client.

[Retour au sommet](#table-des-matières)

### Quelles sont les responsabilités de sécurité des clients selon le modèle de responsabilité partagée AWS ? (Sélectionnez DEUX.)

Selon le **modèle de responsabilité partagée**, les clients AWS

 sont responsables de plusieurs aspects de la sécurité, notamment :

1. **Gérer les données client :** Les clients sont responsables de la protection et de la gestion des données qu'ils stockent sur AWS.
2. **Maintenir l'infrastructure réseau et la virtualisation de l'infrastructure :** C'est une responsabilité d'AWS.
3. **Gérer les informations d'identification et les politiques du compte :** Les clients doivent gérer les accès et les permissions pour garantir la sécurité de leurs comptes AWS.
4. **Maintenir le matériel physique :** C'est une responsabilité d'AWS.
5. **Maintenir tout hyperviseur utilisé sur les instances :** C'est une responsabilité d'AWS.

[Retour au sommet](#table-des-matières)

### Qui est responsable de l'exploitation, de la gestion et du contrôle de la sécurité DE l'infrastructure cloud, selon le modèle de responsabilité partagée AWS ?

Dans le **modèle de responsabilité partagée**, AWS est responsable de l'exploitation, de la gestion et du contrôle de la sécurité de l'infrastructure cloud sous-jacente. Voici les responsabilités :

1. **Organisation de services gérés (MSO) :** Non applicable.
2. **Client :** Les clients sont responsables de la sécurité des données et des applications qu'ils déploient dans le cloud.
3. **Fournisseur de services gérés (MSP) :** Non applicable.
4. **AWS :** AWS est responsable de la sécurité de l'infrastructure physique et du réseau qui soutient les services cloud.

[Retour au sommet](#table-des-matières)

## Module 3 : Gestion des identités et des accès (IAM)

### Quelle affirmation concernant la gestion des identités et des accès (IAM) d'AWS est vraie ?

La gestion des identités et des accès (IAM) d'AWS permet aux organisations de contrôler finement l'accès à leurs ressources AWS. Voici une affirmation vraie concernant IAM :

1. **IAM fournit le cryptage des données au repos et en transit :** Ce n'est pas la fonction principale d'IAM.
2. **Avec IAM, vous pouvez accorder aux principaux un accès granulaire à la console :** IAM permet de définir des politiques d'accès détaillées, accordant des permissions spécifiques aux utilisateurs, groupes et rôles.
3. **IAM fournit l'audit de qui a effectué une action, quelle action ils ont effectuée, et quand ils l'ont effectuée :** AWS CloudTrail fournit cette fonctionnalité.
4. **IAM améliore la sécurité en interdisant la fédération des systèmes d'entreprise tels que Microsoft Active Directory :** Ce n'est pas exact, IAM peut être intégré avec des systèmes de fédération tels que Microsoft Active Directory.

[Retour au sommet](#table-des-matières)

### Quelle option est considérée comme une meilleure pratique pour configurer l'accès à long terme dans la gestion des identités et des accès (IAM) d'AWS ?

Pour une gestion efficace et sécurisée des identités et des accès, il est recommandé de **"attacher des politiques IAM à des groupes IAM, puis assigner des utilisateurs IAM aux groupes IAM"**. Voici pourquoi :

1. **Attacher des politiques IAM à des groupes IAM, puis assigner des utilisateurs IAM aux groupes IAM :** Cette approche simplifie la gestion des permissions en permettant de gérer les accès par groupe plutôt que par utilisateur individuel.
2. **Créer un nouveau groupe avec des conditions plus spécifiques, puis l'assigner à un groupe parent :** Cela peut compliquer la gestion des permissions.
3. **Créer un rôle, appliquer des permissions, puis permettre à vos membres du personnel de supposer ce rôle :** C'est une bonne pratique pour des accès temporaires ou spécifiques, mais pour l'accès à long terme, la gestion par groupes est préférable.
4. **Attacher des politiques IAM aux utilisateurs IAM, puis assigner des utilisateurs IAM à des groupes IAM :** Cette approche peut devenir difficile à gérer à grande échelle.

[Retour au sommet](#table-des-matières)

### Quelle affirmation décrit le mieux un rôle de la gestion des identités et des accès (IAM) d'AWS ?

Un rôle IAM est une identité utilisée pour accorder des permissions à un principal de manière temporaire. Voici la meilleure description :

1. **Un rôle est une personne ou une application qui peut s'authentifier avec un compte AWS :** C'est une description d'un principal, pas d'un rôle.
2. **Un rôle est une identité qui est utilisée pour accorder un ensemble permanent de permissions pour effectuer des demandes de service AWS :** Ce n'est pas exact, les rôles sont temporaires.
3. **Un rôle est une identité qui est utilisée pour accorder un ensemble temporaire de permissions pour effectuer des demandes de service AWS :** Correct, les rôles permettent de déléguer des permissions temporaires à des utilisateurs ou des services.
4. **Un rôle est un document qui définit quelles ressources un utilisateur peut accéder :** Cela décrit une politique, pas un rôle.

[Retour au sommet](#table-des-matières)

### Quelle méthode permettrait de réaliser une authentification multi-facteurs (MFA) ?

L'authentification multi-facteurs (MFA) ajoute une couche supplémentaire de sécurité en exigeant plus d'une forme de vérification avant de permettre l'accès. Voici une méthode courante pour réaliser une MFA :

1. **Exiger une clé d'accès pour s'authentifier par programmation :** Non, cela ne constitue pas une MFA.
2. **Exiger un ID de clé d'accès et une clé d'accès secrète pour s'authentifier par programmation :** Non, cela ne constitue pas une MFA.
3. **Exiger une clé d'accès et un code d'authentification provenant d'un appareil matériel :** Correct, cela correspond à l'utilisation d'une MFA, où un deuxième facteur (code d'authentification) est requis en plus de la clé d'accès.
4. **Exiger un nom d'utilisateur et un mot de passe pour s'authentifier par programmation :** Non, cela ne constitue pas une MFA.

[Retour au sommet](#table-des-matières)

### Quelle affirmation concernant les politiques de gestion des identités et des accès (IAM) d'AWS est vraie ?

Les politiques IAM définissent les permissions accordées aux identités. Voici une affirmation vraie :

1. **Les politiques basées sur les ressources sont attachées aux ressources et accordent des permissions au principal spécifié dans la politique :** Correct, les politiques basées sur les ressources définissent qui peut accéder à la ressource et quelles actions ils peuvent effectuer.
2. **Les politiques basées sur les identités sont attachées aux ressources et accordent des permissions au principal spécifié dans la politique :** Non, elles sont attachées aux identités (utilisateurs, groupes, rôles).
3. **Par défaut, toutes les actions ou ressources qui ne sont pas explicitement refusées par une politique sont autorisées :** Non, par défaut, toutes les actions sont refusées sauf si explicitement autorisées.
4. **Les limites de permissions sont utilisées pour accorder des permissions au sein d'une région AWS spécifique :** Non, elles définissent les limites maximales des permissions qu'un principal peut obtenir, indépendamment des régions.

[Retour au sommet](#table-des-matières)

### Un administrateur a décidé d'utiliser des politiques en ligne pour améliorer la posture de sécurité de son organisation. Quelle affirmation concernant les politiques en ligne est vraie ?

Les politiques en ligne sont attachées directement à une entité principale. Voici une affirmation vraie :

1. **Les politiques en ligne sont des politiques autonomes basées sur l'identité que AWS crée et gère :** Non, AWS ne crée pas automatiquement des politiques en ligne.
2. **Les politiques en ligne sont une forme de politique de contrôle de service (SCP) :** Non, SCP est une politique d'AWS Organizations.
3. **Les politiques en ligne sont une partie intégrante et inhérente d'une entité principale telle qu'un utilisateur, un groupe ou un rôle :** Correct, les politiques en ligne sont directement attachées aux entités et ne sont pas réutilisables.
4. **Les politiques en ligne offrent des fonctionnalités telles que la réutilisabilité, la gestion des changements centralisés et la versioning :** Non, ce sont des caractéristiques des politiques gérées.

[Retour au sommet](#table-des-matières)

### Quelle méthode fournirait la fédération des identités ?

La fédération des identités permet d'utiliser des identités externes pour accéder aux ressources AWS. Voici une méthode pour réaliser cela :

1. **Mettre en œuvre AWS Single Sign-On (SSO), qui prend en charge les interactions sécurisées entre un fournisseur d'identité et un fournisseur de service :** Correct, AWS SSO permet d'intégrer des fournisseurs d'identité externes pour accéder aux ressources AWS.
2. **Mettre en œuvre AWS Organizations et organiser les comptes de manière hiérarchique via l'utilisation d'unités organisationnelles (OU) :** Non, cela concerne la gestion des comptes et des politiques au sein d'une organisation.
3. **Mettre en œuvre une authentification multi-facteurs (MFA) et exiger l'utilisation d'un appareil d'authentification virtuel :** Non, cela concerne la sécurité des identités existantes, pas la fédération.
4. **Mettre en œuvre AWS CloudTrail, qui fournit des enregistrements de journal incluant les informations d'identité des utilisateurs qui demandent des ressources dans votre compte :** Non, cela concerne l'audit des actions, pas la fédération.

[Retour au sommet](#table-des-matières)

### Quel service AWS repose sur des pools d'utilisateurs et des pools d'identités ?

Le service AWS suivant utilise des pools d'utilisateurs et des

 pools d'identités :

1. **AWS CloudTrail, qui fournit des enregistrements de journal incluant les informations d'identité des utilisateurs qui demandent des ressources dans votre compte :** Non, cela concerne l'audit des actions.
2. **Amazon Cognito, qui fournit l'authentification, l'autorisation et la gestion des utilisateurs pour les applications mobiles et web :** Correct, Amazon Cognito utilise des pools d'utilisateurs pour gérer les utilisateurs et des pools d'identités pour fournir des identités temporaires.
3. **AWS Organizations, qui fournit la capacité d'organiser de manière hiérarchique vos actifs en utilisant des unités organisationnelles (OU) :** Non, cela concerne la gestion des comptes et des politiques au sein d'une organisation.
4. **AWS Directory Service, qui fournit la capacité pour les charges de travail et les ressources AWS de utiliser Microsoft Active Directory géré dans le cloud AWS :** Non, cela concerne la gestion des annuaires et des identités Microsoft Active Directory.

[Retour au sommet](#table-des-matières)

### Quelle affirmation concernant AWS Organizations est vraie ?

AWS Organizations permet une gestion centralisée des comptes AWS. Voici une affirmation vraie :

1. **Avec Organizations, vous pouvez attacher des politiques à chaque OU pour la création et l'application de politiques fines :** Correct, AWS Organizations permet d'attacher des politiques de contrôle de service (SCP) aux unités organisationnelles (OU).
2. **Organizations remplace la gestion des identités et des accès (IAM) d'AWS en annulant le contrôle granulaire que IAM fournit au niveau du compte :** Non, IAM reste essentiel pour la gestion des identités et des permissions au sein des comptes AWS.
3. **Organizations utilise des politiques de contrôle de service (SCP) pour accorder des permissions aux comptes membres au sein d'une organisation :** Correct, les SCP permettent de définir des politiques qui restreignent ou permettent certaines actions au sein des comptes membres.
4. **Vous pouvez imbriquer des unités organisationnelles (OU) dans d'autres OU jusqu'à une profondeur de sept niveaux :** Correct, AWS Organizations permet une hiérarchie complexe avec des unités organisationnelles imbriquées.

[Retour au sommet](#table-des-matières)

### Quel service AWS la gestion des identités et des accès (IAM) d'AWS utilise-t-elle pour fournir des informations d'identification de sécurité temporaires pour les rôles ?

Pour fournir des informations d'identification de sécurité temporaires, IAM utilise :

1. **AWS Organizations :** Non, cela concerne la gestion des comptes et des politiques au sein d'une organisation.
2. **AWS CloudTrail :** Non, cela concerne l'audit des actions.
3. **Amazon Cognito :** Non, cela concerne la gestion des utilisateurs pour les applications mobiles et web.
4. **AWS Security Token Service (AWS STS) :** Correct, AWS STS fournit des informations d'identification temporaires pour les rôles et les utilisateurs fédérés.

[Retour au sommet](#table-des-matières)

## Module 5 : Sécurité et conformité

### Quels éléments peuvent être utilisés pour contrôler l'accès à un bucket Amazon S3 ? (Sélectionnez TROIS.)

Pour contrôler l'accès à un bucket Amazon S3, les éléments suivants peuvent être utilisés :

1. **Contrôle d'accès basé sur les attributs (ABAC) :** Correct, ABAC utilise des attributs pour contrôler l'accès aux ressources AWS.
2. **Accès basé sur l'hôte :** Non, ce n'est pas une méthode courante pour S3.
3. **Balises IAM :** Correct, les balises IAM peuvent être utilisées pour contrôler l'accès en fonction des balises attachées aux ressources.
4. **Politiques de gestion des identités et des accès (IAM) d'AWS :** Correct, les politiques IAM peuvent être utilisées pour définir les permissions d'accès aux ressources S3.
5. **Politiques de point d'accès de cloud privé virtuel (VPC) :** Non, cela concerne l'accès au niveau du VPC, pas directement aux buckets S3.
6. **Politiques de bucket S3 :** Correct, les politiques de bucket S3 définissent les permissions d'accès directement sur les buckets.

[Retour au sommet](#table-des-matières)

### Quel mécanisme de contrôle est recommandé pour protéger les données en transit vers Amazon S3 afin de réduire le risque d'accès non autorisé ou de perte ?

Pour protéger les données en transit vers Amazon S3, le mécanisme recommandé est :

1. **Appliquer le contrôle d'accès :** Important, mais cela ne concerne pas directement la protection des données en transit.
2. **Utiliser des listes de contrôle d'accès réseau (ACL) sur le bucket :** Non, cela concerne l'accès aux objets stockés, pas la protection en transit.
3. **Utiliser les points de terminaison du cloud privé virtuel (VPC) d'Amazon pour limiter l'accès au bucket :** Correct, les points de terminaison VPC permettent une connexion privée et sécurisée à S3, réduisant ainsi les risques d'accès non autorisé.
4. **Configurer un groupe de sécurité et ajouter le bucket S3 à celui-ci :** Non, les groupes de sécurité ne s'appliquent pas directement aux buckets S3.

[Retour au sommet](#table-des-matières)

### Amazon S3 Object Lock est une fonctionnalité qui offre une protection pour les scénarios où il est impératif que les données ne soient pas modifiées ou supprimées. Quelles sont les deux façons de gérer la rétention d'objets avec Object Lock ?

Amazon S3 Object Lock offre une protection en utilisant :

1. **Périodes de rétention et blocages légaux :** Correct, ces deux méthodes permettent de gérer la rétention des objets et de les protéger contre la suppression ou la modification.
2. **Périodes de rétention et journaux de flux VPC :** Non, les journaux de flux VPC ne sont pas liés à S3 Object Lock.
3. **Restreindre les buckets publics et bloquer la politique publique :** Non, bien que cela renforce la sécurité, ce n'est pas lié à Object Lock.
4. **Blocages légaux et hiérarchisation intelligente S3 :** Non, hiérarchisation intelligente S3 est une fonctionnalité de gestion du stockage, pas de rétention.

[Retour au sommet](#table-des-matières)

### Quelle fonctionnalité d'Amazon S3 stocke des objets en utilisant le modèle écrire-une-fois-lire-plusieurs (WORM) ?

La fonctionnalité d'Amazon S3 qui utilise le modèle WORM est :

1. **Verrouillage d'objet S3 :** Correct, S3 Object Lock permet de stocker des objets en mode WORM.
2. **Versioning S3 :** Non, bien que cela permette de conserver plusieurs versions, ce n'est pas WORM.
3. **Bloquer l'accès public S3 :** Non, cela concerne les permissions d'accès, pas le modèle WORM.
4. **Bloquer les ACL publiques S3 :** Non, cela concerne les contrôles d'accès, pas le modèle WORM.

[Retour au sommet](#table-des-matières)

### Quel service AWS peut être utilisé pour générer des clés de cryptage protégées par des modules de sécurité matériels validés FIPS 140-2 (HSM) ?

Le service AWS suivant peut générer des clés de cryptage protégées par des HSM validés FIPS 140-2 :

1. **Gestionnaire de secrets AWS :** Non, cela concerne le stockage sécurisé des secrets.
2. **Gestionnaire de certificats AWS (ACM) :** Non, cela concerne la gestion des certificats SSL/TLS.
3. **Autorité de certification privée du gestionnaire de certificats AWS :** Non, cela concerne la gestion des CA privées.
4. **Service de gestion des clés AWS (KMS) :** Correct, KMS peut utiliser des HSM validés FIPS 140-2 pour générer et protéger des clés de cryptage.

[Retour au sommet](#table-des-matières)

### Quels types de cryptage côté serveur (SSE) sont disponibles pour Amazon S3 ? (Sélectionnez TROIS.)

Les types de cryptage côté serveur (SSE) disponibles pour Amazon S3 incluent :

1. **SSE avec des clés gérées par Amazon S3 (SSE-S3) :** Correct, AWS gère les clés de cryptage.
2. **SSE avec des clés Advanced Encryption Standard (AES) 256 bits (SSE-AES) :** Non, ce n'est pas une option spécifique.
3. **SSE avec des clés du service de gestion des clés AWS (SSE-KMS) :** Correct, les clés sont gérées par AWS KMS.
4. **SSE avec des clés fournies par le client (SSE-C) :** Correct, les clients gèrent leurs propres clés de cryptage.
5. **SSE avec des clés gérées par SSL (SSE-SSL) :** Non, ce n'est pas une option.
6. **SSE avec des clés de module de sécurité matériel AWS (SSE-HSM) :** Non, ce n'est pas une option spécifique.

[Retour au sommet](#table-des-matières)

### Quelle affirmation concernant le cryptage côté client est vraie ?

Le cryptage côté client signifie que les données sont chiffrées avant d'être envoyées à AWS. Voici une affirmation vraie :

1. **Les données sont stockées dans leur état chiffré, mais les clés et les algorithmes sont connus par le client et AWS :** Non, AWS ne connaît pas les clés et

 les algorithmes.
2. **L'application chiffre les données après les avoir envoyées à AWS :** Non, le chiffrement est fait avant l'envoi.
3. **Les clés et les algorithmes sont connus uniquement par le client :** Correct, le client garde le contrôle total sur le chiffrement.
4. **Les données sont chiffrées à leur destination par l'application ou le service qui les reçoit :** Non, c'est fait avant l'envoi.

[Retour au sommet](#table-des-matières)

### Quelle affirmation est vraie à propos du cryptage côté serveur dans Amazon S3 avec des clés fournies par le client (SSE-C) ?

Le cryptage côté serveur avec des clés fournies par le client (SSE-C) implique :

1. **Le client conserve le contrôle des clés :** Correct, le client fournit et gère les clés de cryptage.
2. **Les données chiffrées et les clés sont stockées sur des hôtes séparés :** Non, ce n'est pas une exigence spécifique.
3. **SSE-C fournit une piste de vérification qui montre quand la clé a été utilisée et par qui :** Non, ce n'est pas une caractéristique de SSE-C.
4. **Une clé d'enveloppe est utilisée pour une protection supplémentaire :** Non, ce n'est pas spécifique à SSE-C.

[Retour au sommet](#table-des-matières)

### Quel service AWS peut être utilisé pour stocker des éléments, tels que des informations d'identification de base de données, de manière sécurisée sans avoir à les coder en dur dans le code source ou un fichier de configuration ?

Le service AWS suivant peut être utilisé pour cela :

1. **Service de gestion des clés AWS (KMS) :** Non, cela concerne principalement la gestion des clés de cryptage.
2. **Gestionnaire de secrets AWS :** Correct, cela permet de stocker et de gérer des secrets de manière sécurisée.
3. **Autorité de certification privée du gestionnaire de certificats AWS :** Non, cela concerne la gestion des CA privées.
4. **Amazon Macie :** Non, cela concerne la découverte et la protection des données sensibles.

[Retour au sommet](#table-des-matières)

### Un ingénieur en apprentissage automatique souhaite une solution qui découvrira les informations sensibles stockées dans AWS. La solution doit également utiliser le traitement du langage naturel (NLP) pour classifier les données et fournir des insights liés aux affaires. Quel service AWS répondrait à leurs besoins ?

Le service AWS suivant répondrait à ces besoins :

1. **Amazon Macie :** Correct, Macie utilise le NLP pour découvrir, classifier et protéger les données sensibles.
2. **AWS Lambda :** Non, cela concerne l'exécution de code en réponse à des événements.
3. **Gestionnaire de systèmes AWS :** Non, cela concerne la gestion des ressources et des applications AWS.
4. **Amazon Inspector :** Non, cela concerne l'analyse des vulnérabilités.

[Retour au sommet](#table-des-matières)

## Module 6 : Surveillance et journalisation

### Quelle affirmation concernant les journaux et les fichiers journaux est vraie ?

Les journaux et les fichiers journaux jouent un rôle crucial dans la gestion de la sécurité et de la conformité. Voici une affirmation vraie :

1. **Les fichiers journaux peuvent être utilisés pour démontrer la conformité avec les réglementations :** Correct, les journaux peuvent prouver que les pratiques de sécurité et de gestion des données sont respectées.
2. **Les fichiers journaux sont une exigence pour la réponse aux incidents et la remédiation :** Correct, ils sont essentiels pour analyser et répondre aux incidents de sécurité.
3. **Les fichiers journaux sont une violation du Règlement général sur la protection des données (RGPD) de l'Union européenne :** Non, tant qu'ils sont gérés correctement et que la confidentialité est respectée.
4. **Les fichiers journaux ne doivent pas être utilisés pour l'audit en raison de leur nature peu fiable :** Non, les journaux sont cruciaux pour l'audit et la conformité.

[Retour au sommet](#table-des-matières)

### Quelle affirmation concernant la surveillance AWS est vraie ?

La surveillance AWS est un processus continu et essentiel pour la gestion des ressources et des applications. Voici une affirmation vraie :

1. **Amazon CloudWatch est le seul fournisseur de capacités de surveillance :** Non, AWS propose plusieurs outils de surveillance.
2. **AWS CloudTrail permet de surveiller vos ressources et applications en temps réel :** Non, CloudTrail se concentre sur l'audit des actions.
3. **La surveillance est un processus continu :** Correct, la surveillance doit être continue pour être efficace.
4. **Amazon CloudWatch permet d'enregistrer qui fait quoi et quand ils le font :** Non, c'est la fonction de CloudTrail.

[Retour au sommet](#table-des-matières)

### Quelle affirmation concernant AWS CloudTrail est vraie ?

AWS CloudTrail enregistre les actions effectuées sur votre compte AWS. Voici une affirmation vraie :

1. **CloudTrail peut être intégré dans des applications en utilisant l'API :** Correct, CloudTrail peut être intégré via l'API.
2. **CloudTrail vous aide à activer la gouvernance et à gérer la conformité en désactivant l'audit opérationnel et de risque de votre compte AWS :** Non, il aide en activant l'audit.
3. **CloudTrail enregistre les actions effectuées par un utilisateur, un rôle ou un service AWS sous forme de pistes :** Correct, il enregistre les actions sous forme de pistes de logs.
4. **CloudTrail n'enregistre pas les actions effectuées dans la console de gestion AWS :** Non, il enregistre les actions de la console.

[Retour au sommet](#table-des-matières)

### Quelle information AWS CloudTrail NE capture-t-elle PAS ?

AWS CloudTrail capture de nombreuses informations, mais voici ce qu'il ne capture pas :

1. **L'emplacement d'origine de la demande :** Correct, il capture cela.
2. **L'heure de l'appel API :** Correct, il capture cela.
3. **L'heure d'origine de la demande dans votre heure locale :** Non, il capture l'heure en UTC.
4. **Le nom de l'API appelée :** Correct, il capture cela.

[Retour au sommet](#table-des-matières)

### Une application web utilise une flotte d'instances Amazon EC2 pour des actifs dynamiques et statiques. Les instances EC2 sont dans un sous-réseau privé, derrière un équilibreur de charge qui est dans un sous-réseau public à l'intérieur du VPC. Quels journaux de service fourniraient le PLUS d'informations sur la façon dont les utilisateurs utilisent l'application web ?

Pour obtenir le plus d'informations sur l'utilisation de l'application web, utilisez :

1. **Journaux d'accès Elastic Load Balancing (ELB) :** Correct, ces journaux fournissent des informations détaillées sur les requêtes HTTP/S.
2. **Journaux d'accès Amazon S3 :** Non, cela concerne l'accès aux objets S3.
3. **Journaux AWS Trusted Advisor :** Non, cela concerne les recommandations de sécurité et d'optimisation.
4. **Journaux de flux Amazon VPC :** Non, bien que utiles, ils ne fournissent pas autant de détails que les journaux ELB.

[Retour au sommet](#table-des-matières)

### Quelle affirmation concernant Amazon CloudWatch est vraie ?

Amazon CloudWatch est un service de surveillance et de gestion des ressources AWS. Voici une affirmation vraie :

1. **CloudWatch scanne continuellement les charges de travail AWS pour détecter les vulnérabilités logicielles et l'exposition non intentionnelle au réseau :** Non, cela relève d'Amazon Inspector.
2. **CloudWatch peut être utilisé pour détecter un comportement anormal et invoquer d'autres services pour prendre d'autres mesures :** Correct, il peut être utilisé pour définir des alarmes et automatiser des réponses.
3. **CloudWatch est utilisé pour créer des silos de données pour se protéger contre la visibilité et la résolution des problèmes à l'échelle du système :** Non, il est utilisé pour la surveillance centralisée.
4. **CloudWatch ne peut être utilisé que pour collecter des métriques dans l'environnement AWS Cloud :** Non, il peut collecter des métriques de ressources on-premises également.

[Retour au sommet](#table-des-matières)

### AWS CloudTrail et Amazon CloudWatch remplissent des fonctions spécifiques. Quelle fonction est indicative de CloudWatch ?

Voici la fonction spécifique de CloudWatch :

1. **Vous aide à déterminer qui a effectué une action non autorisée :** Non, cela relève de CloudTrail.
2. **Est utile pour détecter un comportement anormal, définir des alarmes et découvrir des insights :** Correct, cela décrit les fonctions de CloudWatch.
3. **Est utile pour l'audit de conformité, l'analyse de sécurité et le dépannage :** Non, cela relève de CloudTrail.
4. **Surveille en continu les activités des utilisateurs :** Non, cela relève de CloudTrail.

[Retour au sommet](#table-des-matières)

### Quelle est la première étape pour utiliser les contrôles détectifs que AWS fournit ?

La première étape pour utiliser les contrôles détectifs est :

1. **Déterminer le coût des services détectifs fournis par AWS :** Non, bien que pertinent, ce n'est pas la première étape.
2. **Configurer la journalisation des services et des applications :** Correct, il faut d'abord configurer la journalisation pour capturer les données nécessaires.
3. **Analyser centralement les journaux :** Non, cela vient après la configuration de la journalisation.
4

. **Définir vos exigences pour les journaux, les alertes et les métriques :** Non, cela vient après la configuration de la journalisation.

[Retour au sommet](#table-des-matières)

### Quel service AWS évalue votre compte en utilisant des vérifications qui identifient des moyens d'optimiser votre infrastructure AWS, d'améliorer la sécurité et les performances, de réduire les coûts et de surveiller les quotas de service ?

Le service AWS suivant fait cela :

1. **Amazon EventBridge :** Non, cela concerne l'intégration d'événements.
2. **AWS Trusted Advisor :** Correct, Trusted Advisor fournit des vérifications et des recommandations pour optimiser votre infrastructure AWS.
3. **AWS Config :** Non, cela concerne la gestion des configurations et des conformités.
4. **AWS Security Hub :** Non, cela centralise les alertes et les résultats de sécurité.

[Retour au sommet](#table-des-matières)

### Quel service de journalisation et de surveillance AWS est un service de bus d'événements sans serveur qui peut connecter vos applications avec des données provenant de diverses sources ?

Le service AWS suivant correspond à cette description :

1. **AWS Trusted Advisor :** Non, cela fournit des recommandations de sécurité et d'optimisation.
2. **AWS Security Hub :** Non, cela centralise les alertes et les résultats de sécurité.
3. **Amazon EventBridge :** Correct, EventBridge est un bus d'événements sans serveur qui peut connecter différentes applications.
4. **AWS Config :** Non, cela concerne la gestion des configurations et des conformités.

[Retour au sommet](#table-des-matières)

## Module 7 : Sécurité et gestion dans AWS

### Quel service AWS est un service de surveillance et d'évaluation continue qui fournit un inventaire des ressources AWS et enregistre les modifications apportées à leur configuration ?

Le service AWS suivant répond à cette description :

1. **AWS Shield :** Non, cela concerne la protection DDoS.
2. **Amazon Inspector :** Non, cela concerne l'analyse des vulnérabilités.
3. **Amazon Simple Notification Service (Amazon SNS) :** Non, cela concerne la messagerie.
4. **AWS Config :** Correct, Config fournit un inventaire et enregistre les modifications.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite obtenir de l'aide pour gérer des opérations complexes et chronophages, telles que la mise à jour des logiciels et le routage des tickets de support. Quel service AWS répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **Amazon Detective :** Non, cela concerne l'investigation de sécurité.
2. **AWS Config :** Non, cela concerne la gestion des configurations.
3. **AWS Step Functions :** Correct, Step Functions aide à orchestrer les workflows complexes.
4. **Amazon Simple Notification Service (Amazon SNS) :** Non, cela concerne la messagerie.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite s'assurer qu'il optimise son infrastructure AWS et améliore également la performance de la sécurité. Quelle option répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **AWS Shield :** Non, cela concerne la protection DDoS.
2. **AWS Trusted Advisor :** Correct, Trusted Advisor fournit des recommandations pour optimiser l'infrastructure et améliorer la sécurité.
3. **AWS Systems Manager :** Non, cela concerne la gestion des opérations.
4. **Amazon Macie :** Non, cela concerne la découverte et la protection des données sensibles.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite avoir des informations supplémentaires sur leurs applications et collecter des données opérationnelles sous forme de journaux, de métriques et d'événements. Quel service AWS répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **AWS CloudFormation :** Non, cela concerne la gestion de l'infrastructure en tant que code.
2. **Amazon CloudWatch :** Correct, CloudWatch collecte et surveille les métriques, journaux et événements.
3. **Amazon Inspector :** Non, cela concerne l'analyse des vulnérabilités.
4. **Amazon Detective :** Non, cela concerne l'investigation de sécurité.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite améliorer la gestion de leurs vulnérabilités. Ils aimeraient utiliser un service qui scanne en continu les instances Amazon EC2 pour détecter les vulnérabilités logicielles et l'exposition non intentionnelle au réseau. Quel service AWS répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **Amazon Inspector :** Correct, Inspector scanne en continu les instances EC2 pour détecter les vulnérabilités.
2. **Amazon Detective :** Non, cela concerne l'investigation de sécurité.
3. **AWS Shield :** Non, cela concerne la protection DDoS.
4. **Amazon Macie :** Non, cela concerne la découverte et la protection des données sensibles.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite effectuer des vérifications d'évaluation continues sur leurs ressources. Le but est de vérifier que ces ressources respectent leurs propres politiques de sécurité, les meilleures pratiques de l'industrie et les normes de conformité. Quel service AWS répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **Amazon CloudWatch :** Non, cela concerne la surveillance des métriques et des journaux.
2. **AWS CloudFormation :** Non, cela concerne la gestion de l'infrastructure en tant que code.
3. **AWS Config :** Correct, Config effectue des vérifications d'évaluation continues pour assurer la conformité.
4. **AWS CloudTrail :** Non, cela concerne l'audit des actions.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite augmenter la protection de leur réseau et éliminer toute activité potentiellement non autorisée ou malveillante dans leur environnement AWS. Quel service AWS répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **Amazon Macie :** Non, cela concerne la découverte et la protection des données sensibles.
2. **AWS CloudTrail :** Non, cela concerne l'audit des actions.
3. **Liste de contrôle d'accès réseau (ACL) :** Non, cela concerne les contrôles d'accès.
4. **Amazon GuardDuty :** Correct, GuardDuty détecte les activités non autorisées ou malveillantes.

[Retour au sommet](#table-des-matières)

### Quelles sont les meilleures pratiques pour gérer un incident ? (Sélectionnez DEUX.)

Pour gérer un incident efficacement, les meilleures pratiques incluent :

1. **Exécuter du code à la demande :** Non, cela ne concerne pas directement la gestion des incidents.
2. **Créer un modèle décrivant toutes les ressources AWS que vous souhaitez utiliser :** Non, cela concerne la gestion de l'infrastructure.
3. **Identifier le personnel clé, les ressources externes et les outils :** Correct, cela aide à réagir rapidement et efficacement.
4. **Surveiller et évaluer en continu les services qui fournissent un inventaire des ressources AWS :** Non, bien que pertinent, ce n'est pas directement lié à la gestion des incidents.
5. **Automatiser les capacités de confinement :** Correct, cela permet de réduire l'impact des incidents rapidement.

[Retour au sommet](#table-des-matières)

### Un administrateur souhaite mettre en œuvre un niveau de protection contre les attaques par déni de service distribué (DDoS). Quel service AWS répondrait à leur besoin ?

Le service AWS suivant répond à cette description :

1. **AWS Shield :** Correct, Shield offre une protection contre les attaques DDoS.
2. **AWS Firewall Manager :** Non, cela concerne la gestion centralisée des règles de sécurité.
3. **Amazon Inspector :** Non, cela concerne l'analyse des vulnérabilités.
4. **Amazon Detective :** Non, cela concerne l'investigation de sécurité.

[Retour au sommet](#table-des-matières)

### Quelle définition décrit le mieux une réponse aux incidents ?

Voici la définition qui décrit le mieux une réponse aux incidents :

1. **Ensemble de solutions de surveillance fiables, évolutives et flexibles :** Non, cela concerne la surveillance.
2. **Capture de l'image disque ou de la configuration telle quelle d'un système d'exploitation :** Non, bien que pertinent, ce n'est pas la définition complète d'une réponse aux incidents.
3. **Tout événement déclenchant une alarme dans Amazon CloudWatch :** Non, cela concerne la détection d'incidents.
4. **Ensemble de politiques et de procédures de sécurité de l'information pouvant être utilisées pour identifier, contenir et éliminer les cyberattaques :** Correct, cela décrit une réponse aux incidents de manière complète.

[Retour au sommet](#table-des-matières)
