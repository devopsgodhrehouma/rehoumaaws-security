# Module 2 Vérification des connaissances

#### 1. Quel est l'avantage de l'élasticité dans le cloud ?
- Minimiser les exigences de stockage en réduisant les activités de journalisation et d'audit.
- Réduire le temps nécessaire pour rendre de nouvelles ressources informatiques disponibles aux développeurs, passant de semaines à minutes.
- **Créer des systèmes qui évoluent en fonction de la capacité requise selon les variations de la demande.**
- Échanger des dépenses fixes contre des dépenses variables.

#### 2. Quel est le modèle de tarification que les clients AWS peuvent utiliser pour payer les ressources selon leurs besoins ?
- Payer à la réservation
- **Payer à l'utilisation**
- Payer à la désactivation
- Payer à l'achat

#### 3. Quelle est une méthode qu'une entreprise pourrait utiliser pour garantir une haute disponibilité lors d'une attaque de sécurité ?
- Audits réguliers
- **Mise à l'échelle automatique**
- Surveillance des ressources
- Contrôle d'accès

#### 4. Quel principe de sécurité traite de la surveillance, de l'alerte et de l'audit des actions et des changements de l'environnement en temps réel ?
- Protéger les données en transit et au repos
- Sécuriser toutes les couches
- Appliquer le principe du moindre privilège
- **Activer la traçabilité**

#### 5. Quelle est une meilleure pratique pour l'automatisation qui peut aider à fournir une infrastructure sécurisée fiable et répétable ?
- Utiliser des contrôles de détection
- Utiliser le chiffrement et les contrôles d'accès
- **Implémenter l'infrastructure en tant que code**
- Implémenter une solution de gestion des journaux

#### 6. Quelles options sont des caractéristiques du principe du moindre privilège ? (Sélectionnez DEUX.)
- Utiliser différents services AWS
- Surveiller les actions et les changements
- **Faire respecter la séparation des tâches**
- Utiliser le chiffrement et les contrôles d'accès
- **Accorder l'accès uniquement au besoin**

#### 7. Quelles options sont des principes de sécurité basés sur le pilier de sécurité du cadre AWS bien architecturé ? (Sélectionnez TROIS.)
- Utiliser la mise à l'échelle automatique
- **Préparer les événements de sécurité**
- **Démocratiser les technologies avancées**
- **Automatiser les meilleures pratiques de sécurité**
- Concevoir vos systèmes pour une haute disponibilité
- Protéger les données en transit et au repos

#### 8. La sécurité et la conformité sont des responsabilités partagées entre AWS et le client. Quelle est une responsabilité AWS ?
- Configurer des groupes de sécurité
- Gérer les logiciels d'application
- **Appliquer des correctifs à l'infrastructure réseau**
- Gérer les données client

#### 9. Quelles sont les responsabilités de sécurité des clients selon le modèle de responsabilité partagée AWS ? (Sélectionnez DEUX.)
- **Gérer les données client**
- Maintenir l'infrastructure réseau et la virtualisation de l'infrastructure
- **Gérer les informations d'identification et les politiques du compte**
- Maintenir le matériel physique
- Maintenir tout hyperviseur utilisé sur les instances

#### 10. Qui est responsable de l'exploitation, de la gestion et du contrôle de la sécurité DE l'infrastructure cloud, selon le modèle de responsabilité partagée AWS ?
- Organisation de services gérés (MSO)
- Client
- Fournisseur de services gérés (MSP)
- **AWS**

---

### Module 3 Vérification des connaissances

#### 1. Quelle affirmation concernant la gestion des identités et des accès (IAM) d'AWS est vraie ?
- IAM fournit le cryptage des données au repos et en transit.
- **Avec IAM, vous pouvez accorder aux principaux un accès granulaire à la console.**
- IAM fournit l'audit de qui a effectué une action, quelle action ils ont effectuée, et quand ils l'ont effectuée.
- IAM améliore la sécurité en interdisant la fédération des systèmes d'entreprise tels que Microsoft Active Directory.

#### 2. Quelle option est considérée comme une meilleure pratique pour configurer l'accès à long terme dans la gestion des identités et des accès (IAM) d'AWS ?
- **Attacher des politiques IAM à des groupes IAM, puis assigner des utilisateurs IAM aux groupes IAM.**
- Créer un nouveau groupe avec des conditions plus spécifiques, puis l'assigner à un groupe parent.
- Créer un rôle, appliquer des permissions, puis permettre à vos membres du personnel de supposer ce rôle.
- Attacher des politiques IAM aux utilisateurs IAM, puis assigner des utilisateurs IAM à des groupes IAM.

#### 3. Quelle affirmation décrit le mieux un rôle de la gestion des identités et des accès (IAM) d'AWS ?
- Un rôle est une personne ou une application qui peut s'authentifier avec un compte AWS.
- Un rôle est une identité qui est utilisée pour accorder un ensemble permanent de permissions pour effectuer des demandes de service AWS.
- **Un rôle est une identité qui est utilisée pour accorder un ensemble temporaire de permissions pour effectuer des demandes de service AWS.**
- Un rôle est un document qui définit quelles ressources un utilisateur peut accéder.

#### 4. Quelle méthode permettrait de réaliser une authentification multi-facteurs (MFA) ?
- Exiger une clé d'accès pour s'authentifier par programmation.
- Exiger un ID de clé d'accès et une clé d'accès secrète pour s'authentifier par programmation.
- **Exiger une clé d'accès et un code d'authentification provenant d'un appareil matériel.**
- Exiger un nom d'utilisateur et un mot de passe pour s'authentifier par programmation.

#### 5. Quelle affirmation concernant les politiques de gestion des identités et des accès (IAM) d'AWS est vraie ?
- Les politiques basées sur les ressources sont attachées aux ressources et accordent des permissions au principal spécifié dans la politique.
- Les politiques basées sur les identités sont attachées aux ressources et accordent des permissions au principal spécifié dans la politique.
- **Par défaut, toutes les actions ou ressources qui ne sont pas explicitement refusées par une politique sont autorisées.**
- Les limites de permissions sont utilisées pour accorder des permissions au sein d'une région AWS spécifique.

#### 6. Un administrateur a décidé d'utiliser des politiques en ligne pour améliorer la posture de sécurité de son organisation. Quelle affirmation concernant les politiques en ligne est vraie ?
- Les politiques en ligne sont des politiques autonomes basées sur l'identité que AWS crée et gère.
- Les politiques en ligne sont une forme de politique de contrôle de service (SCP).
- **Les politiques en ligne sont une partie intégrante et inhérente d'une entité principale telle qu'un utilisateur, un groupe ou un rôle.**
- Les politiques en ligne offrent des fonctionnalités telles que la réutilisabilité, la gestion des changements centralisés et la versioning.

#### 7. Quelle méthode fournirait la fédération des identités ?
- **Mettre en œuvre AWS Single Sign-On (SSO), qui prend en charge les interactions sécurisées entre un fournisseur d'identité et un fournisseur de service.**
- Mettre en œuvre AWS Organizations et organiser les comptes de manière hiérarchique via l'utilisation d'unités organisationnelles (OU).
- Mettre en œuvre une authentification multi-facteurs (MFA) et exiger l'utilisation d'un appareil d'authentification virtuel.
- Mettre en œuvre AWS CloudTrail, qui fournit des enregistrements de journal incluant les informations d'identité des utilisateurs qui demandent des ressources dans votre compte.

#### 8. Quel service AWS repose sur des pools d'utilisateurs et des pools d'identités ?
- AWS CloudTrail, qui fournit des enregistrements de journal incluant les informations d'identité des utilisateurs qui demandent des ressources dans votre compte.
- **Amazon Cognito, qui fournit l'authentification, l'autorisation et la gestion des utilisateurs pour les applications mobiles et web.**
- AWS Organizations, qui fournit la capacité d'organiser de manière hiérarchique vos actifs en utilisant des unités organisationnelles (OU).
- AWS Directory Service, qui fournit la capacité pour les charges de travail et les ressources AWS de utiliser Microsoft Active Directory géré dans le cloud AWS.

#### 9. Quelle affirmation concernant AWS Organizations est vraie ?
- **Avec Organizations, vous pouvez attacher des politiques à chaque OU pour la création et l'application de politiques fines.**
- Organizations remplace la gestion des identités et des accès (IAM) d'AWS en annulant le contrôle granulaire que IAM fournit au niveau du compte.
- Organizations utilise des politiques de contrôle de service (SCP) pour accorder des permissions aux comptes membres au sein d'une organisation.
- Vous pouvez imbriquer des unités organisationnelles (OU) dans d'autres OU jusqu'à une profondeur de sept niveaux.

#### 10. Quel service AWS la gestion des identités et des accès (IAM) d'AWS utilise-t-elle pour fournir des informations d'identification de sécurité temporaires pour les rôles ?
- AWS Organizations
- AWS CloudTrail
- Amazon Cognito
- **AWS Security Token Service (AWS STS)**

---

### Module 5 Vérification des connaissances

#### 1. Quels éléments peuvent être utilisés pour contrôler l'accès à un bucket Amazon S3 ? (Sélectionnez TROIS.)
- Contrôle d'accès basé sur les attributs (ABAC)
- Accès basé sur l'hôte
- Balises IAM
- **Politiques de gestion des identités et des accès (IAM) d'AWS**
- **Politiques de point d'accès de cloud privé virtuel (VPC)**
- **Politiques de bucket S3**

#### 2. Quel mécanisme de contrôle est recommandé pour protéger les données en transit vers Amazon S3 afin de réduire le risque d'accès non autorisé ou de perte ?
- Appliquer le contrôle d'accès.
- **Utiliser des listes de contrôle d'accès réseau (ACL) sur le bucket.**
- Utiliser les points de terminaison du cloud privé virtuel (VPC) d'Amazon pour limiter l'accès au bucket.
- Configurer un groupe de sécurité et ajouter le bucket S3 à celui-ci.

#### 3. Amazon S3 Object Lock est une fonctionnalité qui offre une protection pour les scénarios où il est impératif que les données ne soient pas modifiées ou supprimées. Quelles sont les deux façons de gérer la rétention d'objets avec Object Lock ?
- **Périodes de rétention et blocages légaux**
- Périodes de rétention et journaux de flux VPC
- Restreindre les buckets publics et bloquer la politique publique
- Blocages légaux et hiérarchisation intelligente S3

#### 4. Quelle fonctionnalité d'Amazon S3 stocke des objets en utilisant le modèle écrire-une-fois-lire-plusieurs (WORM) ?
- **Verrouillage d'objet S3**
- Versioning S3
- Bloquer l'accès public S3
- Bloquer les ACL publiques S3

#### 5. Quel service AWS peut être utilisé pour générer des clés de cryptage protégées par des modules de sécurité matériels validés FIPS 140-2 (HSM) ?
- Gestionnaire de secrets AWS
- Gestionnaire de certificats AWS (ACM)
- Autorité de certification privée du gestionnaire de certificats AWS
- **Service de gestion des clés AWS (KMS)**

#### 6. Quels types de cryptage côté serveur (SSE) sont disponibles pour Amazon S3 ? (Sélectionnez TROIS.)
- **SSE avec des clés gérées par Amazon S3 (SSE-S3)**
- SSE avec des clés Advanced Encryption Standard (AES) 256 bits (SSE-AES)
- **SSE avec des clés du service de gestion des clés AWS (SSE-KMS)**
- **SSE avec des clés fournies par le client (SSE-C)**
- SSE avec des clés gérées par SSL (SSE-SSL)
- SSE avec des clés de module de sécurité matériel AWS (SSE-HSM)

#### 7. Quelle affirmation concernant le cryptage côté client est vraie ?
- Les données sont stockées dans leur état chiffré, mais les clés et les algorithmes sont connus par le client et AWS.
- L'application chiffre les données après les avoir envoyées à AWS.
- **Les clés et les algorithmes sont connus uniquement par le client.**
- Les données sont chiffrées à leur destination par l'application ou le service qui les reçoit.

#### 8. Quelle affirmation est vraie à propos du cryptage côté serveur dans Amazon S3 avec des clés fournies par le client (SSE-C) ?
- **Le client conserve le contrôle des clés.**
- Les données chiffrées et les clés sont stockées sur des hôtes séparés.
- SSE-C fournit une piste de vérification qui montre quand la clé a été utilisée et par qui.
- Une clé d'enveloppe est utilisée pour une protection supplémentaire.

#### 9. Quel service AWS peut être utilisé pour stocker des éléments, tels que des informations d'identification de base de données, de manière sécurisée sans avoir à les coder en dur dans le code source ou un fichier de configuration ?
- Service de gestion des clés AWS (KMS)
- **Gestionnaire de secrets AWS**
- Autorité de certification privée du gestionnaire de certificats AWS
- Amazon Macie

#### 10. Un ingénieur en apprentissage automatique souhaite une solution qui découvrira les informations sensibles stockées dans AWS. La solution doit également utiliser le traitement du langage naturel (NLP) pour classifier les données et fournir des insights liés aux affaires. Quel service AWS répondrait à leurs besoins ?
- **Amazon Macie**
- AWS Lambda
- Gestionnaire de systèmes AWS
- Amazon Inspector
