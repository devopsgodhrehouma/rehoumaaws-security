# https://www.youtube.com/playlist?list=PLVQzKSZl3hU5i4edsBByq_kmGhKfkd75q

### Laboratoire 3.1 : Utilisation des politiques basées sur les ressources pour sécuriser un seau S3

#### Aperçu et objectifs du laboratoire

Dans ce laboratoire, vous apprendrez à configurer des permissions en utilisant les politiques basées sur les identités et les ressources d'AWS Identity and Access Management (IAM), telles que les politiques de seau Amazon Simple Storage Service (Amazon S3). Vous apprendrez également comment les politiques IAM et les politiques de ressources définissent les permissions d'accès.

Après avoir terminé ce laboratoire, vous devriez être capable de :

- Reconnaître comment utiliser les politiques basées sur les identités IAM et les politiques basées sur les ressources pour définir un contrôle d'accès granulaire aux services et ressources AWS.
- Décrire comment un utilisateur IAM peut assumer un rôle IAM pour obtenir différentes permissions d'accès à un compte AWS.
- Expliquer comment les politiques de seau S3 et les politiques basées sur les identités IAM attribuées aux utilisateurs et rôles IAM affectent ce que les utilisateurs peuvent voir ou modifier dans différents services AWS dans la console de gestion AWS.

#### Durée

Ce laboratoire nécessite environ 60 minutes pour être complété.

#### Restrictions de service AWS

Dans cet environnement de laboratoire, l'accès aux services AWS et aux actions de service peut être limité à ceux nécessaires pour compléter les instructions du laboratoire. Vous pourriez rencontrer des erreurs si vous essayez d'accéder à d'autres services ou de réaliser des actions au-delà de celles décrites dans ce laboratoire.

#### Scénario

Le diagramme suivant montre l'architecture qui a été créée pour vous dans AWS au début du laboratoire.

L'environnement de laboratoire dispose de trois seaux Amazon S3 préconfigurés : bucket1, bucket2 et bucket3. L'environnement dispose également d'un rôle IAM préconfiguré, qui permet l'accès à certains seaux et à leurs objets lorsque le rôle est assumé. Vous allez analyser différentes politiques pour mieux comprendre comment elles contrôlent votre niveau d'accès.

#### Tâche 1 : Accéder à la console en tant qu'utilisateur IAM

- En haut de ces instructions, choisissez Démarrer le laboratoire.
- La session de laboratoire commence.
- Un chronomètre s'affiche en haut de la page et montre le temps restant dans la session.
- Attendez que l'icône circulaire à droite du lien AWS dans le coin supérieur gauche devienne verte. Lorsque l'environnement de laboratoire est prêt, le panneau Détails AWS s'affiche également.
- Connectez-vous en tant qu'utilisateur IAM nommé devuser :
  - Choisissez le lien Détails AWS en haut de la page.
  - Copiez la valeur IAMUserLoginURL et chargez-la dans un nouvel onglet de navigateur.
  - Pour le nom d'utilisateur IAM, entrez devuser.
  - Pour le mot de passe, entrez la valeur IAMUserPassword du panneau Détails AWS sur la page des instructions du laboratoire.
  - Choisissez Connexion.
- La console de gestion AWS s'affiche.
- Disposez l'onglet de la console de gestion AWS de manière à ce qu'il s'affiche à côté de ces instructions. Idéalement, vous devriez pouvoir voir les deux onglets du navigateur en même temps pour suivre plus facilement les étapes du laboratoire.

#### Tâche 2 : Tenter d'accéder à un niveau de lecture aux services AWS

Maintenant que vous êtes connecté à la console en tant qu'utilisateur IAM nommé devuser, vous allez explorer le niveau d'accès que vous avez à quelques services AWS, notamment Amazon Elastic Compute Cloud (Amazon EC2), Amazon S3 et IAM.

- Ouvrez la console Amazon EC2 :
  - Dans le menu Services, choisissez Compute > EC2.
  - Dans le volet de navigation de gauche, choisissez Tableau de bord EC2.
  - De nombreux messages d'erreur API s'affichent. Cela est attendu.
  - Tentez certaines actions dans la console Amazon EC2 :
    - Dans le volet de navigation de gauche, choisissez Instances.
    - Dans la liste des instances, un message s'affiche Vous n'êtes pas autorisé à effectuer cette opération.
    - Choisissez Lancer des instances.
    - Faites défiler vers le bas et choisissez la liste déroulante Nom de la paire de clés.
    - Un message s'affiche Vous n'êtes pas autorisé à effectuer cette opération.
    - Dans le panneau Résumé à droite, choisissez Annuler.

Pour explorer ce que vous pouvez accéder dans la console Amazon S3, dans le menu Services, choisissez Stockage > S3.

- Trois seaux sont répertoriés. Les noms des seaux sont uniques, mais un nom de seau contient bucket1, un autre contient bucket2, et le troisième contient bucket3.
- Dans la liste des seaux, remarquez que la colonne Accès affiche le message Permissions insuffisantes pour les trois seaux. Cela est attendu.

#### Tâche 3 : Analyser la politique basée sur les identités appliquée à l'utilisateur IAM

Vous avez observé comment l'utilisateur IAM devuser ne peut pas accéder à certaines informations et actions dans les consoles Amazon S3 et Amazon EC2. Dans cette tâche, vous allez examiner les détails de la politique IAM appliquée à devuser pour comprendre pourquoi vous ne pouvez pas effectuer ces actions.

- Accédez à la console IAM et observez les paramètres d'utilisateur et de groupe :
  - Dans le menu Services, choisissez Sécurité, identité et conformité > IAM.
  - Sur la page du tableau de bord IAM, remarquez que vous n'avez pas les permissions pour afficher certaines parties de la page. Les deux messages indiquent User: arn:aws:iam:::user/devuser is not authorized to perform: iam:GetAccountSummary on resource: *. Cela est attendu.
  - Dans le volet de navigation de gauche, choisissez Groupes d'utilisateurs.
  - Choisissez le nom du groupe DeveloperGroup.
  - Dans l'onglet Utilisateurs, remarquez que devuser est membre de ce groupe IAM.
  - Choisissez l'onglet Permissions.
  - Remarquez qu'une politique IAM nommée DeveloperGroupPolicy est attachée à ce groupe IAM.

Note : Lorsqu'une politique est attachée à un groupe, la politique s'applique à tous les utilisateurs IAM membres du groupe. Par conséquent, cette politique régit actuellement votre accès à la console, car vous êtes connecté en tant que devuser, qui est membre de ce groupe IAM.

- Examinez les détails de la politique IAM :
  - Dans la partie inférieure de la page, choisissez l'icône plus à gauche de DeveloperGroupPolicy pour afficher les détails de la politique.
  - Examinez les détails de la politique JSON et rappelez-vous du niveau d'accès que vous aviez pour Amazon EC2 et Amazon S3 dans la tâche précédente.
  - Remarquez que la politique ne permet aucune action Amazon EC2.
  - Remarquez les actions IAM que la politique permet. Lorsque vous avez accédé au tableau de bord IAM, vous avez vu un message indiquant que vous n'aviez pas l'autorisation iam:GetAccountSummary. Cette action n'est pas autorisée dans ce document de politique. Cependant, de nombreuses permissions IAM de niveau lecture sont accordées. Par exemple, vous pouvez examiner les détails de cette politique.
  - Remarquez les actions Amazon S3 que la politique permet. Aucune action liée aux objets n'est accordée, mais certaines actions liées aux seaux sont autorisées.

- Enregistrez la politique dans un fichier sur votre ordinateur :
  - Pour copier la politique formatée en JSON dans votre presse-papiers, choisissez Copier.
  - Ouvrez un éditeur de texte sur votre ordinateur local et collez la politique que vous venez de copier.
  - Enregistrez le document de politique sous le nom DeveloperGroupPolicy.json à un emplacement sur votre ordinateur dont vous vous souviendrez.

#### Tâche 4 : Tenter un accès de niveau écriture aux services AWS

Toute action que vous tentez lorsque vous interagissez avec un service AWS est un appel d'API, que vous utilisiez la console, AWS Command Line Interface (AWS CLI) ou AWS Software Development Kits (SDKs). Tous les appels d'API tentés sont enregistrés dans les journaux d'événements AWS CloudTrail.

Dans cette tâche, vous tenterez de faire deux appels d'API qui nécessitent un accès de niveau écriture dans Amazon S3. La première action consiste à créer un seau S3, et la deuxième action consiste à télécharger un objet dans ce seau. Après avoir tenté les deux tâches, vous analyserez de nouveau la politique attachée au groupe IAM pour analyser pourquoi vous avez pu ou non effectuer les appels d'API spécifiques.

- Tentez de créer un seau S3 :
  - Accédez à la console Amazon S3.
  - Choisissez Créer un seau.
  - Pour Nom du seau, entrez vos initiales suivies d'un nombre aléatoire à quatre chiffres ; par exemple, zba1234.
  - Pour Région AWS, choisissez US East (N. Virginia) us-east-1.
  - Examinez les paramètres, puis choisissez Créer un seau en bas de la page.
  - Vous avez créé un seau S3 avec succès.

- Accédez au seau et tentez de télécharger un objet :
  - Choisissez le nom du seau que vous venez de créer.
  - Choisissez Télécharger, puis choisissez Ajouter des fichiers.
  - Parcourez et choisissez le fichier DeveloperGroupPolicy.json que vous avez enregistré plus tôt.
  - Choisissez Télécharger.
  - Un message s'affiche Téléchargement échoué.
  - Dans l'onglet Fichiers et dossiers dans la partie inférieure de la page, dans la colonne Er

reur, choisissez le lien Accès refusé.
  - Le message indique Vous n'avez pas les permissions pour télécharger des fichiers et des dossiers.
  - Choisissez Fermer.
  - Depuis le fil d'Ariane dans le coin supérieur gauche de la page, choisissez Amazon S3.

- Examinez les détails de la politique pour l'accès à Amazon S3 :
  - Retournez à l'éditeur de texte où vous avez copié le document DeveloperGroupPolicy.json.
  - Examinez les détails de la politique pour comprendre pourquoi vous avez pu créer un seau S3 mais pas télécharger des objets dans celui-ci.
  - Le document de référence des autorisations de service fournit une liste des actions que chaque service AWS prend en charge. Pour des informations sur les actions Amazon S3, ouvrez la page de documentation IAM, puis ouvrez le document de référence des autorisations de service. Dans le volet de navigation de gauche, développez Actions, ressources et clés de condition, puis choisissez Amazon S3. Dans la section Actions définies par Amazon S3, la table répertorie toutes les actions Amazon S3 possibles qui peuvent être accordées ou refusées, ainsi qu'une description de l'action.

#### Tâche 5 : Assumer un rôle IAM et examiner une politique basée sur les ressources

Dans cette tâche, vous allez essayer d'accéder à bucket1 et bucket2 tout en étant connecté en tant qu'utilisateur IAM devuser. Vous allez également essayer d'accéder aux seaux en utilisant un rôle préconfiguré dans le cadre de la configuration du laboratoire.

- Essayez de télécharger un objet à partir des seaux créés pendant la configuration du laboratoire :
  - Dans la console Amazon S3, choisissez le nom du seau qui contient bucket1.
  - Sélectionnez Image2.jpg, puis choisissez Télécharger.
  - Une page d'erreur Accès refusé apparaît.
  - Pour retourner à la console Amazon S3, choisissez le bouton Précédent de votre navigateur.
  - Depuis le fil d'Ariane dans le coin supérieur gauche de la page, choisissez Amazon S3.
  - Essayez de télécharger le fichier Image1.jpg de bucket2.
  - Vous recevez la même erreur.
  - Pour retourner à la console Amazon S3, choisissez le bouton Précédent de votre navigateur.

Analyse : Comme montré dans le diagramme suivant, avec les permissions accordées par l'appartenance au DeveloperGroup, vous avez pu créer un nouveau seau. Cependant, vous ne pouvez pas accéder aux objets dans bucket1 ou bucket2.

- Assumer le rôle IAM BucketsAccessRole dans la console :
  - Dans le coin supérieur droit de la page, choisissez devuser, puis choisissez Changer de rôle.
  - Si la page Changer de rôle s'affiche, choisissez Changer de rôle.
  - Configurez les éléments suivants :
    - Compte : Entrez la valeur AccountID du panneau Détails AWS sur la page des instructions du laboratoire.
    - Rôle : Entrez BucketsAccessRole.
  - Choisissez Changer de rôle.
  - Vous avez assumé le rôle IAM nommé BucketsAccessRole, qui a été préconfiguré pour ce laboratoire.
  - Essayez de télécharger un objet depuis Amazon S3 à nouveau :
    - Dans la console Amazon S3, choisissez le nom du seau qui contient bucket1.
    - Sélectionnez Image2.jpg, puis choisissez Télécharger.
    - Ouvrez le fichier pour vérifier que le fichier a été téléchargé.

Analyse : Le téléchargement a réussi, ce qui signifie que la politique ou les politiques appliquées au BucketsAccessRole permettent l'action s3:GetObject sur bucket1.

- Tester l'accès IAM avec BucketsAccessRole :
  - Accédez à la console IAM.
  - Dans le volet de navigation de gauche, choisissez Groupes d'utilisateurs.
  - Analyse : Un message d'erreur s'affiche. Vous n'avez plus les permissions pour afficher la page des groupes d'utilisateurs IAM car BucketsAccessRole n'a pas l'action iam:ListGroups appliquée.

- Assumer à nouveau le rôle devuser et tester l'accès à la page des groupes d'utilisateurs :
  - Dans le coin supérieur droit de la page, choisissez BucketsAccessRole, puis choisissez Revenir en arrière.
  - Dans le volet de navigation de gauche, choisissez Groupes d'utilisateurs à nouveau.
  - Analyse : Maintenant que vous avez désassumé le rôle BucketsAccessRole, vous avez les permissions assignées à l'utilisateur IAM devuser (grâce à son appartenance au DeveloperGroup). Vous pouvez à nouveau afficher la page des groupes d'utilisateurs.

- Analyser la politique IAM associée au BucketsAccessRole :
  - Dans le volet de navigation de gauche, choisissez Rôles.
  - Recherchez BucketsAccessRole et choisissez le nom du rôle lorsqu'il apparaît.
  - Choisissez la flèche à gauche de ListAllBucketsPolicy.
  - Cette politique accorde la même action s3:ListAllMyBuckets à chaque ressource. Cette permission vous permet de voir tous les seaux S3 lorsque vous assumez BucketsAccessRole.
  - Choisissez la flèche à gauche de GrantBucket1Access.

Analyse : Cette politique permet les actions s3:GetObject, s3:ListObjects et s3:ListBucket. Remarquez que cette politique n'accorde pas l'accès s3:PutObject. Les actions autorisées ne sont accordées que pour des ressources spécifiques, bucket1 et tous les objets qu'il contient (indiqués par /*). L'astérisque (*) est un caractère générique, ce qui signifie qu'il correspondrait à n'importe quelle valeur.

- Enregistrez une copie de la politique GrantBucket1Access sur votre ordinateur :
  - Placez votre curseur au début de la ligne 1 dans les détails de la politique et sélectionnez toutes les lignes de code (jusqu'à la ligne 17).
  - Copiez la politique formatée en JSON dans votre presse-papiers.
  - Ouvrez un nouveau fichier texte sur votre ordinateur et collez la politique que vous venez de copier.
  - Enregistrez le document de politique sous le nom GrantBucket1Access.json à un emplacement sur votre ordinateur dont vous vous souviendrez.

- Complétez votre analyse des détails du BucketsAccessRole :
  - Faites défiler la page vers le haut et choisissez l'onglet Relations de confiance.
  - Remarquez que l'utilisateur IAM devuser dans ce compte AWS est répertorié comme une entité de confiance pouvant assumer ce rôle.
  - Remarquez que le numéro de compte qui apparaît dans le coin supérieur droit de la console (après devuser) correspond au numéro de compte dans la liste des entités de confiance (sans les tirets).

Note : AWS Security Token Service (AWS STS) fournira des informations d'identification temporaires à toute entité de confiance qui demande d'assumer le rôle. Cette politique de confiance fait confiance à un utilisateur IAM dans le même compte. Cependant, une politique de confiance pourrait être configurée pour faire confiance à un ou plusieurs principaux, même dans d'autres comptes AWS. Des exemples d'autres principaux sont les services AWS, les rôles IAM et les utilisateurs IAM.

- Assumer le BucketsAccessRole et essayer de télécharger une image dans bucket2 :
  - Pour assumer de nouveau le BucketsAccessRole, dans le coin supérieur droit de la page, choisissez devuser.
  - Sous Historique des rôles, choisissez BucketsAccessRole.
  - Accédez à la console Amazon S3.
  - Choisissez le nom du seau qui contient bucket2.
  - Remarquez que ce seau n'a pas encore de fichier Image2.jpg.
  - Choisissez Télécharger, puis choisissez Ajouter des fichiers.
  - Parcourez et choisissez le fichier Image2.jpg que vous avez téléchargé plus tôt de bucket1.
  - Choisissez Télécharger.
  - Le fichier se télécharge avec succès.
  - Choisissez Fermer.

Analyse : Après avoir assumé le BucketsAccessRole, vous avez accédé avec succès à bucket1 pour télécharger un objet. Vous avez ensuite téléchargé le même objet dans bucket2.

Après avoir inspecté les politiques attachées au BucketsAccessRole, vous savez que les permissions Amazon S3 accordées à ce rôle étaient limitées à bucket1, comme montré dans le diagramme suivant.

La politique GrantBucket1Access est attachée au BucketsAccessRole. Lorsque le rôle est assumé, l'utilisateur peut accéder aux objets dans bucket1 et télécharger des objets dans bucket2.

Alors, comment avez-vous pu télécharger un objet dans bucket2 ? La raison deviendra claire dans la tâche suivante.

#### Tâche 6 : Comprendre les politiques basées sur les ressources

Dans cette tâche, vous allez inspecter la politique de seau associée à bucket2.

- Observez les détails de la politique de seau appliquée à bucket2 :
  - Sur la page des détails de bucket2, choisissez l'onglet Permissions.
  - Dans la section Politique de seau, examinez la politique appliquée à bucket2.
  - La politique comporte deux déclarations.
  - Le premier ID de déclaration (SID) est S3Write. Le principal est le rôle IAM BucketsAccessRole que vous avez assumé. Ce rôle est autorisé à appeler les actions s3:GetObject et s3:PutObject sur la ressource, qui est bucket2.
  - Le second SID est ListBucket. Le principal est BucketsAccessRole. Ce rôle est autorisé à appeler l'action s3:ListBucket sur la ressource, qui est bucket2.

Analyse : Vous devriez maintenant avoir une meilleure compréhension de la manière dont les politiques basées sur les ressources (comme les politiques de seau S3) et les politiques basées sur les rôles (politiques associées aux rôles IAM) peuvent interagir et être utilisées ensemble.

Dans ce laboratoire, les politiques basées sur les rôles attachées au rôle IAM BucketsAccessRole accordaient les actions s3:GetObject et s3:ListBucket sur bucket1 et les objets qu'il contient. Ces politiques basées sur les rôles n'autorisaient pas explicitement l'accès à bucket2 ; cependant, elles ne le refusaient pas non plus explicitement.

Le diagramme suivant montre comment les politiques appliquées à l'utilisateur IAM, au rôle IAM et au seau ont déterminé les actions que vous avez pu effectuer.

BucketsAccessRole a accès à bucket1 grâce à une politique basée sur les rôles et a accès à bucket2 grâce à une politique basée sur les ressources.

Ensuite, tout en assumant toujours le rôle BucketsAccessRole, vous avez essayé de télécharger un objet dans bucket2 et vous avez pu le faire. Cela semblait étrange en fonction des politiques IAM que vous avez examinées. Cependant, après avoir examiné la politique basée sur les ressources (dans ce cas, une politique de seau) attachée au seau, votre accès avait du sens. Cette politique de seau accorde l'accès, y compris l'action s3:PutObject, à bucket2 pour le principal BucketsAccessRole.

#### Tâche de défi

Votre objectif pour cette tâche de défi est de trouver un moyen de télécharger le fichier Image2.jpg dans bucket3.

- Essayez de télécharger le fichier en tant que devuser sans rôle assumé :
  - Désassumez le rôle BucketsAccessRole.
  - Tentez de télécharger Image2.jpg, que vous avez téléchargé depuis bucket1 plus tôt dans ce laboratoire, dans bucket3.
  - Le téléchargement échoue.
  - Vérifiez si une politique de seau est associée à bucket3. Cela pourrait vous donner une indication sur la manière de réaliser cette tâche.
  - Vous ne pouvez pas voir la politique de seau.

- Assumez le rôle BucketsAccessRole et essayez les actions de l'étape précédente :
  - Pouvez-vous télécharger un fichier dans bucket3 ?
  - Pouvez-vous maintenant voir la politique de seau ? Examinez les détails de la politique de seau. Avez-vous une idée de la manière de télécharger Image2.jpg dans bucket3 ?
  - Avez-vous réussi à télécharger le fichier ? Si oui, félicitations !

#### Soumettre votre travail

Pour enregistrer votre progression, choisissez Soumettre en haut de ces instructions.

Lorsque vous y êtes invité, choisissez Oui.

Après quelques minutes, le panneau des notes s'affiche et vous montre combien de points vous avez obtenus pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez Notes en haut de ces instructions.

#### Laboratoire terminé

Félicitations ! Vous avez terminé le laboratoire.

En haut de cette page, choisissez Terminer le laboratoire, puis choisissez Oui pour confirmer que vous souhaitez terminer le laboratoire.

Un panneau de message indique que le laboratoire est en train de se terminer.

Pour fermer le panneau, choisissez Fermer dans le coin supérieur droit.

---

© 2022, Amazon Web Services, Inc. et ses affiliés. Tous droits réservés. Ce travail ne peut pas être reproduit ou redistribué, en tout ou en partie, sans l'autorisation écrite préalable d'Amazon Web Services, Inc. La copie commerciale, le prêt ou la vente sont interdits.
