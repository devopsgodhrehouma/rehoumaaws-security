# Correction : 
- https://www.youtube.com/watch?v=fpzvdHFcCJs&list=PLVQzKSZl3hU5i4edsBByq_kmGhKfkd75q&index=2&ab_channel=DavidDai


# Lab 4.1 : Sécurisation des ressources VPC en utilisant des groupes de sécurité

### Aperçu du laboratoire et objectifs

Les groupes de sécurité sont des pare-feu virtuels attachés aux instances Amazon Elastic Compute Cloud (Amazon EC2). Les règles de groupe de sécurité définissent quel trafic est autorisé à entrer ou sortir d'une instance. Dans ce laboratoire, vous devrez configurer des règles d'accès pour les instances EC2.

Après avoir terminé ce laboratoire, vous devriez être capable de :

- Examiner les groupes de sécurité pour déterminer quel trafic est autorisé.
- Modifier les groupes de sécurité appliqués aux instances EC2.
- Créer de nouveaux groupes de sécurité.
- Mettre à jour les règles entrantes des groupes de sécurité en suivant le principe du moindre privilège.
- Comprendre comment les groupes de sécurité peuvent référencer d'autres groupes de sécurité.
- Configurer une liste de contrôle d'accès réseau (ACL) pour bloquer le trafic sur un port TCP spécifique.
- Se connecter à une instance dans un sous-réseau privé en utilisant SSH.
- Se connecter à une instance dans un sous-réseau privé en utilisant AWS Systems Manager Session Manager.

### Durée

Ce laboratoire nécessitera environ 90 minutes pour être complété.

### Restrictions de service AWS

Dans cet environnement de laboratoire, l'accès aux services AWS et aux actions de service peut être restreint à ceux nécessaires pour compléter les instructions du laboratoire. Vous pouvez rencontrer des erreurs si vous essayez d'accéder à d'autres services ou de réaliser des actions au-delà de celles décrites dans ce laboratoire.

### Scénario

L'environnement de laboratoire comprend un cloud privé virtuel (VPC) entièrement fonctionnel avec plusieurs sous-réseaux, des tables de routage, une passerelle Internet, une passerelle NAT et plusieurs groupes de sécurité. De plus, trois instances EC2 Linux ont été lancées dans différents sous-réseaux : deux dans des sous-réseaux publics et une dans un sous-réseau privé. Les instances dans les sous-réseaux publics sont configurées comme serveurs proxy pour transmettre le trafic au serveur d'application dans le sous-réseau privé. Vous vous connecterez aux instances proxy en utilisant un navigateur web pour tester la connectivité au serveur d'application privé afin de vérifier comment les groupes de sécurité restreignent l'accès.

Le diagramme suivant montre l'architecture qui a été créée pour vous dans le compte AWS au début du laboratoire.

### Diagramme de l'architecture au début du laboratoire

### Accès à la console de gestion AWS

1. En haut de ces instructions, choisissez **Démarrer le laboratoire**.
    - La session de laboratoire démarre.
    - Un chronomètre s'affiche en haut de la page et indique le temps restant dans la session.
    - Astuce : Pour actualiser la durée de la session à tout moment, choisissez **Démarrer le laboratoire** à nouveau avant que le chronomètre atteigne 00:00.
    - Avant de continuer, attendez que l'icône circulaire à droite du lien **AWS** dans le coin supérieur gauche devienne verte. Lorsque l'environnement de laboratoire est prêt, le panneau de détails AWS s'affichera également.

2. Pour vous connecter à la console de gestion AWS, choisissez le lien **AWS** dans le coin supérieur gauche, au-dessus de la fenêtre de terminal.
    - Un nouvel onglet du navigateur s'ouvre et se connecte à la console.
    - Astuce : Si un nouvel onglet du navigateur ne s'ouvre pas, une bannière ou une icône se trouve généralement en haut de votre navigateur avec le message que votre navigateur empêche le site d'ouvrir des fenêtres pop-up. Choisissez la bannière ou l'icône, puis choisissez **Autoriser les pop-ups**.

### Tâche 1 : Analyse des paramètres de ressources du VPC et du sous-réseau privé

Dans cette première tâche, vous explorerez les ressources configurées dans le LabVPC, afin de vous familiariser avec ses paramètres réseau actuels.

1. Dans la console de gestion AWS, dans la boîte de recherche à droite de **Services**, recherchez et choisissez **VPC** pour ouvrir la console Amazon Virtual Private Cloud (Amazon VPC).

2. Analysez le LabVPC et les sous-réseaux configurés dans celui-ci.
    - Dans le panneau de navigation, choisissez **Vos VPCs**.
    - La région AWS que vous observez actuellement dispose de deux VPCs. L'un est le VPC par défaut, qui a une plage CIDR de 172.31.0.0/16. L'autre est nommé LabVPC et a une plage CIDR de 10.0.0.0/16. Vous utiliserez le LabVPC dans ce laboratoire.
    - Dans le panneau de navigation, choisissez **Subnets** (Sous-réseaux).
    - Dans la liste des sous-réseaux, choisissez l'en-tête de la colonne **VPC** pour organiser les sous-réseaux par VPC, comme montré dans l'image suivante.
    - Remarquez que LabVPC a trois sous-réseaux définis en son sein. Ils sont nommés PrivateSubnet, PublicSubnetA, et PublicSubnetB, et chacun a une plage CIDR d'adresses IPv4 privées unique assignée.

3. Analysez les détails de routage qui s'appliquent aux instances EC2 dans le PrivateSubnet et changez le nom de la table de routage.
    - Sélectionnez la case à cocher pour PrivateSubnet, puis dans le panneau inférieur, choisissez l'onglet **Route table** (Table de routage).
    - Au-dessus de la liste des routes, choisissez le lien pour la table de routage associée à ce sous-réseau.
    - Choisissez l'onglet **Routes**.
    - Remarquez que toutes les adresses IP dans la plage CIDR du LabVPC (10.0.0.0/16) seront routées vers des cibles locales au sein du VPC. Tout autre trafic (0.0.0.0/0) sera routé vers une passerelle NAT.
    - Note : Si le trafic 0.0.0.0/0 pour ce sous-réseau était plutôt routé vers une passerelle Internet, alors vous sauriez qu'il s'agit d'un sous-réseau public avec un accès direct à Internet. Au lieu de cela, il route le trafic réseau vers une passerelle NAT.
    - Choisissez l'onglet **Tags**, puis choisissez **Manage tags** (Gérer les tags).
    - Changez le nom de la table de routage de **changeme** à **Private** et choisissez **Save** (Sauvegarder).
    - Choisissez l'onglet **Routes**, puis choisissez le lien **nat-xxxxxx**, qui est la cible pour le trafic non local.
    - Dans l'onglet **Details** (Détails), remarquez que cette passerelle NAT fonctionne dans PublicSubnetA. Par conséquent, tout trafic vers Internet initié par des instances dans le PrivateSubnet sera routé vers une passerelle NAT dans PublicSubnetA.
    - Analyse détaillée : La passerelle NAT fournit un accès à Internet pour les instances dans le sous-réseau privé. Cependant, les réponses à de telles demandes des ordinateurs sur Internet ne connaîtront pas l'adresse IP du serveur dans le sous-réseau privé qui fait la demande. Au lieu de cela, la passerelle Internet routiera les réponses reçues vers la passerelle NAT, et la passerelle NAT routera les réponses au serveur demandeur. De cette manière, les serveurs dans le sous-réseau privé ne sont pas directement connectés à Internet et leurs adresses IP ne sont pas connues des ordinateurs sur Internet. Cependant, les serveurs dans le sous-réseau privé peuvent toujours communiquer avec Internet et recevoir des réponses.

4. Observez les détails concernant l'instance EC2 AppServer, qui se trouve dans le PrivateSubnet, et son groupe de sécurité associé.
    - Dans la console, dans la boîte de recherche à droite de **Services**, recherchez et choisissez **EC2**.
    - Important : Faites un clic droit sur le service EC2 dans les résultats pour ouvrir la console Amazon EC2 dans un nouvel onglet du navigateur.
    - Astuce : Gardez les onglets du navigateur de la console Amazon VPC et de la console Amazon EC2 ouverts tout au long de ce laboratoire. Vous utiliserez les deux dans les étapes ultérieures.
    - Dans la console Amazon EC2, dans le panneau de navigation, choisissez **Instances**.
        - Sélectionnez l'instance AppServer, et dans l'onglet **Details** (Détails) :
            - Remarquez que cette instance fonctionne dans le PrivateSubnet.
            - Remarquez également qu'elle a une adresse IPv4 privée, mais elle n'a pas d'adresse IPv4 publique.
            - Note : Une adresse IPv4 privée est une adresse IP qui n'est pas accessible sur Internet. Les adresses IPv4 privées peuvent être utilisées pour la communication entre instances dans le même VPC.
    - Choisissez l'onglet **Security** (Sécurité), puis choisissez le lien du groupe de sécurité associé à cette instance. Le nom du groupe de sécurité contient AppServerSG.
    - Choisissez l'onglet **Inbound rules** (Règles entrantes), et remarquez qu'une règle a été définie. Le trafic HTTP sur le port TCP 80 avec une source de 0.0.0.0/0 (partout) est autorisé.
    - Astuce : Vous devrez peut-être faire défiler la table des règles entrantes vers la droite pour voir tous ces détails.
    - Note :
        - Cela

 signifie que toute instance qui a le groupe de sécurité AppServerSG attaché peut recevoir des connexions de tout ordinateur qui tente de communiquer avec elle sur le port TCP 80.
        - Par défaut, lorsque vous créez un groupe de sécurité, il n'a pas de règles entrantes, ce qui signifie que le trafic réseau entrant n'est pas autorisé. Cependant, ce groupe de sécurité a une règle entrante définie.
        - Les groupes de sécurité sont à état. Cela signifie que si une instance envoie une demande, le trafic de réponse à cette demande est autorisé à entrer indépendamment des règles de groupe de sécurité entrantes.
    - Choisissez l'onglet **Outbound rules** (Règles sortantes).
    - Remarquez que tout le trafic sortant est autorisé. C'est le paramètre par défaut pour les groupes de sécurité.

Maintenant que vous avez analysé les paramètres du sous-réseau privé, vous examinerez les mêmes détails pour les sous-réseaux publics.

### Tâche 2 : Analyse des paramètres de ressources du sous-réseau public

Dans cette tâche, vous analyserez les paramètres réseau, de routage et de sécurité des sous-réseaux publics du VPC.

1. Dans la console Amazon VPC, analysez les détails de routage qui affectent les instances EC2 dans PublicSubnetA.
    - Retournez à l'onglet du navigateur où la console Amazon VPC est ouverte.
    - Dans le panneau de navigation, choisissez **Subnets** (Sous-réseaux), puis sélectionnez PublicSubnetA.
    - Note : Si nécessaire, décochez la case pour PrivateSubnet. Si plus d'un élément est sélectionné dans la liste, l'onglet **Details** (Détails) et d'autres onglets ne s'afficheront pas en dessous. Gardez cela à l'esprit tout au long du laboratoire.
    - Choisissez l'onglet **Route table** (Table de routage).
    - Remarquez que cette table de routage de sous-réseau public route le trafic 0.0.0.0/0 vers une passerelle Internet.
    - Note : Ce sous-réseau est appelé sous-réseau public parce qu'il est configuré pour router le trafic non local vers une passerelle Internet.
    - Choisissez le lien **igw-xxxxx**, qui est la cible pour le trafic non local.
    - Dans l'onglet **Details** (Détails), remarquez que cette passerelle Internet est associée au LabVPC.

2. Observez les détails concernant l'instance ProxyServer1, qui fonctionne dans PublicSubnetA, et son groupe de sécurité associé.
    - Retournez à l'onglet du navigateur où la console Amazon EC2 est ouverte.
    - Dans le panneau de navigation, choisissez **Instances**.
    - Sélectionnez l'instance ProxyServer1, puis choisissez l'onglet **Details** (Détails).
    - Remarquez que, contrairement à l'instance AppServer qui fonctionne dans un sous-réseau privé, ce serveur fonctionne dans un sous-réseau public et a une adresse IPv4 publique.
    - Choisissez l'onglet **Security** (Sécurité).
    - Remarquez qu'un groupe de sécurité contenant ProxySG dans son nom est associé à cette instance EC2. Comme vous pouvez le voir dans la section **Inbound rules** (Règles entrantes), le groupe de sécurité permet le trafic entrant sur le port TCP 80 depuis n'importe quelle source.

3. Observez les détails concernant l'instance ProxyServer2, qui fonctionne dans PublicSubnetB, et son groupe de sécurité associé.
    - Sélectionnez l'instance ProxyServer2.
    - Remarquez que ce serveur fonctionne dans PublicSubnetB et a une adresse IPv4 publique.
    - Choisissez l'onglet **Security** (Sécurité). Remarquez qu'un groupe de sécurité contenant ProxySG2 dans son nom est associé à cette instance EC2.

4. Ajoutez une nouvelle règle entrante au groupe de sécurité ProxySG2.
    - Choisissez le lien pour le groupe de sécurité ProxySG2.
    - Choisissez l'onglet **Inbound rules** (Règles entrantes), puis choisissez **Edit inbound rules** (Modifier les règles entrantes).
    - Choisissez **Add rule** (Ajouter une règle), et configurez comme suit :
        - Type : Choisissez **HTTP**.
        - Source : Choisissez **Anywhere-IPv4** (Partout-IPv4).
    - Choisissez **Save rules** (Sauvegarder les règles).
    - Note : Chaque groupe de sécurité peut être configuré différemment, même si vous venez de configurer les paramètres des règles entrantes pour ProxySG2 pour correspondre à ceux trouvés dans ProxySG. Cela signifie que vous pouvez configurer indépendamment comment chaque instance ou groupe d'instances peut être accédé.
    - Le diagramme suivant montre le routage du sous-réseau mis à jour. ProxyServer1 est associé à ProxySG, tandis que ProxyServer2 est associé à ProxySG2. Cependant, les deux groupes de sécurité ont maintenant les mêmes règles entrantes.

Note : Lorsque vous associez un groupe de sécurité à une instance, vous l'assignez en fait à une interface réseau élastique sur l'instance. De nombreuses instances EC2 ont une interface réseau, mais une instance peut avoir plusieurs interfaces réseau. Vous pouvez attacher un groupe de sécurité différent à chaque interface réseau sur une instance pour un comportement d'accès différent selon la connexion réseau.

Dans la prochaine tâche, vous testerez la connectivité à une page web qui est hébergée sur l'AppServer. Plus tard dans le laboratoire, vous modifierez les règles des groupes de sécurité et changerez les instances auxquelles elles sont attachées.

### Tâche 3 : Test de la connectivité HTTP à partir des instances EC2 publiques

Dans cette tâche, vous vous connecterez aux différentes instances provisionnées dans le cadre de votre environnement de laboratoire. Actuellement, les trois groupes de sécurité sont configurés pour autoriser les connexions sur le port TCP 80 à partir de toutes les sources de trafic.

Les serveurs proxy sont situés dans des sous-réseaux publics, accessibles depuis Internet. Le serveur d'application est situé dans un sous-réseau privé et n'est accessible que depuis les serveurs situés à l'intérieur du VPC, soit dans un sous-réseau public, soit dans un sous-réseau privé. Pour vous connecter au serveur web de l'instance AppServer, vous vous connecterez d'abord à l'un des serveurs proxy, qui sont tous deux configurés pour transférer le trafic TCP sur le port 80 vers l'AppServer.

1. Accédez à l'adresse IP publique de ProxyServer1 pour tester le chargement d'une page web hébergée sur l'AppServer.
    - Dans la console Amazon EC2, dans le panneau de navigation, choisissez **Instances**, puis sélectionnez l'instance ProxyServer1.
    - Dans l'onglet **Details** (Détails) ci-dessous, copiez l'adresse IPv4 publique, puis collez-la dans un nouvel onglet du navigateur.
    - Le site web qui se charge est en réalité hébergé sur le serveur d'application. Vous pouvez y accéder car ProxyServer1 est configuré pour transférer tout le trafic TCP du port 80 vers l'AppServer, et les groupes de sécurité ProxySG et AppServerSG sont configurés pour autoriser le trafic entrant.

2. Utilisez la même méthode que celle décrite dans l'étape précédente pour tester le chargement d'une page web en accédant à l'adresse IPv4 publique de ProxyServer2.
    - Le site web qui se charge est le même que celui que vous avez ouvert lorsque vous avez accédé à l'adresse IP publique de ProxyServer1. Cela confirme que ProxyServer1 et ProxyServer2 ont tous deux accès au serveur web fonctionnant sur l'instance AppServer.

3. Fermez les deux onglets où le site web du serveur d'application est ouvert.
    - Note : Gardez les onglets du navigateur de la console Amazon VPC et de la console Amazon EC2 ouverts pour les utiliser dans les étapes ultérieures.

Dans la prochaine tâche, vous mettrez à jour le groupe de sécurité AppServerSG pour restreindre l'accès à l'AppServer.

### Tâche 4 : Restriction de l'accès HTTP en utilisant une adresse IP

Dans cette tâche, vous mettrez à jour le groupe de sécurité AppServerSG pour n'autoriser l'accès qu'à partir d'une seule adresse IP interne, comme indiqué dans le diagramme suivant. Après la mise à jour, vous testerez à nouveau l'accès.

1. Copiez l'adresse IPv4 privée de ProxyServer1, dont vous aurez besoin pour compléter l'étape suivante.
    - Vous pourriez trouver cette valeur dans la console Amazon EC2. Cependant, pour plus de commodité, vous pouvez accéder à cette et d'autres valeurs d'environnement dont vous aurez besoin dans ce laboratoire en choisissant le lien **AWS Details** au-dessus de ces instructions.
    - Copiez la valeur ProxyServer1PrivateIP dans votre presse-papiers.
    - Note : L'adresse IP commence par 10.0.1.

2. Modifiez les règles entrantes du groupe de sécurité AppServerSG.
    - Dans la console Amazon EC2, dans le panneau de navigation, choisissez **Security Groups** (Groupes de sécurité).
    - Note : Vous pouvez accéder aux groupes de sécurité à partir des consoles Amazon VPC et Amazon EC2.
    - Sélectionnez le groupe de sécurité AppServerSG.
    - Choisissez l'onglet **Inbound rules** (Règles entrantes), puis choisissez **Edit inbound

 rules** (Modifier les règles entrantes).
    - Modifiez la règle HTTP comme suit :
        - Dans le champ **Source**, supprimez 0.0.0.0/0 en choisissant l'icône **X**.
        - Ensuite, collez la valeur ProxyServer1PrivateIP à partir des instructions du laboratoire dans le champ **Source**.
        - Après avoir collé, ajoutez /32 à la fin de l'adresse IP.
    - Choisissez **Save rules** (Sauvegarder les règles).
    - Vous avez maintenant mis à jour le groupe de sécurité attaché au serveur d'application pour qu'il n'autorise que le trafic TCP sur le port 80 provenant de ProxyServer1.

3. Testez à nouveau l'accès au site web en chargeant l'adresse IP publique de ProxyServer1 dans un onglet de navigateur.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer1PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Note : Si vous avez gardé l'onglet du navigateur ouvert lors du test précédent, effectuez un rafraîchissement complet de l'onglet du navigateur pour vous assurer que le site web ne se charge pas depuis le cache local.
    - Le site web qui se charge est à nouveau le serveur d'application.
    - Vous pouvez toujours accéder à la page web du serveur d'application depuis ProxyServer1 car le serveur continue de transférer le trafic, et le groupe de sécurité AppServerSG reconnaît la source de la demande comme étant ProxyServer1.

4. Testez à nouveau l'accès au site web en utilisant l'adresse IP publique de ProxyServer2.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer2PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Le site web ne se charge pas. Si vous attendez assez longtemps (ce n'est pas une obligation), après environ 2 minutes, votre navigateur affichera une erreur de délai d'attente de connexion. C'est attendu car le groupe de sécurité de l'AppServer n'autorise le trafic que depuis l'adresse IP assignée à ProxyServer1 et rejette le trafic de toute autre source.
    - Note : Si le site web se charge, effectuez un rafraîchissement complet de l'onglet du navigateur pour vous assurer que le site web ne se charge pas depuis le cache local.

5. Fermez les deux onglets où le site web du serveur d'application est ouvert.
    - Note : Gardez les onglets du navigateur de la console Amazon VPC et de la console Amazon EC2 ouverts pour les utiliser dans les étapes ultérieures.

Dans cette tâche, vous avez modifié un groupe de sécurité pour n'autoriser l'accès entrant qu'à partir d'une seule adresse IP source. Dans la prochaine tâche, vous modifierez le groupe de sécurité AppServerSG pour autoriser le trafic provenant de n'importe quelle instance EC2 dans le LabVPC ayant un groupe de sécurité spécifique attaché.

### Tâche 5 : Extension de l'accès HTTP restreint en référencent un groupe de sécurité

Vous avez décidé de transformer ProxyServer2 en un autre hôte proxy pour créer de la redondance. Quelle est la manière la plus simple de donner à ce serveur et à tout autre serveur futur les mêmes permissions d'accès actuellement assignées à ProxyServer1 ?

Vous pourriez utiliser une adresse IP comme source dans un groupe de sécurité comme démontré dans la tâche précédente. Cependant, vous devrez coder en dur l'adresse IP de chaque instance qui agira comme serveur proxy dans les règles entrantes du groupe de sécurité. La manière la plus simple et la plus sécurisée de déployer les mêmes permissions d'accès pour plusieurs hôtes est de référencer le nom d'un groupe de sécurité comme source entrante plutôt que des adresses IP individuelles.

Jusqu'à présent, le laboratoire s'est concentré sur le groupe de sécurité associé au serveur d'application. Maintenant, vous allez examiner le groupe de sécurité assigné à l'hôte ProxyServer2. Vous pouvez référencer le groupe de sécurité ProxySG comme source entrante pour les connexions HTTP au serveur d'application. Cela signifie que toute instance à laquelle est assigné le groupe de sécurité ProxySG se verra accorder les permissions pour envoyer du trafic HTTP sur le port 80 au serveur d'application.

Le diagramme suivant reflète comment les groupes de sécurité et le routage des sous-réseaux seront configurés après avoir complété cette tâche.

1. Modifiez les règles entrantes d'AppServerSG une fois de plus pour que la source autorisée pour le trafic HTTP soit le groupe de sécurité ProxySG au lieu de l'adresse IP interne de ProxyServer1.
    - Dans la console Amazon EC2, choisissez **Security Groups** (Groupes de sécurité).
    - Sélectionnez le groupe AppServerSG.
    - Choisissez l'onglet **Inbound rules** (Règles entrantes), puis choisissez **Edit inbound rules** (Modifier les règles entrantes).
    - Supprimez la règle entrante existante que vous avez modifiée plus tôt.
    - Choisissez **Add rule** (Ajouter une règle), et configurez comme suit :
        - Type : Choisissez **HTTP**.
        - Source : Choisissez **Custom** (Personnalisé).
        - Dans le champ **Source**, entrez **sg**.
        - Lorsque la liste déroulante des groupes de sécurité existants apparaît, choisissez le groupe de sécurité ProxySG.
    - Choisissez **Save rules** (Sauvegarder les règles).
    - Vous avez maintenant mis à jour le groupe de sécurité attaché au serveur d'application pour qu'il n'autorise que le trafic provenant des instances avec ProxySG attaché.

2. Ajustez le groupe de sécurité attaché à l'instance ProxyServer2.
    - Toujours dans la console Amazon EC2, dans le panneau de navigation, choisissez **Instances**.
    - Sélectionnez l'instance ProxyServer2.
    - Choisissez le menu **Actions** (Actions) dans le coin supérieur droit, puis choisissez **Security > Change security groups** (Sécurité > Modifier les groupes de sécurité).
    - Supprimez le groupe de sécurité ProxySG2.
    - Dans la boîte de recherche, recherchez et choisissez le groupe de sécurité ProxySG.
    - Choisissez **Add security group** (Ajouter un groupe de sécurité), puis choisissez **Save** (Sauvegarder).
    - Maintenant que vous avez mis à jour la configuration, testez pour voir quels hôtes peuvent accéder à l'instance du serveur d'application.

3. Testez l'accès au site web depuis ProxyServer1.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer1PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Le site web se charge.

4. Testez l'accès au site web depuis ProxyServer2, qui est maintenant dans le même groupe de sécurité que ProxyServer1.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer2PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Le site web se charge.
    - Note : Si le site web ne se charge pas, effectuez un rafraîchissement complet de l'onglet du navigateur.

5. Fermez les deux onglets où le site web du serveur d'application est ouvert.
    - Note : Gardez les onglets du navigateur de la console Amazon VPC et de la console Amazon EC2 ouverts pour les utiliser dans les étapes ultérieures.

Dans cette tâche, vous avez mis à jour le groupe de sécurité AppServerSG pour autoriser le trafic provenant de toute instance associée au groupe de sécurité ProxySG. Ensuite, vous avez mis à jour le groupe de sécurité attaché à l'instance ProxyServer2 pour être ProxySG, qui est également attaché à l'instance ProxyServer1. Par conséquent, les deux serveurs proxy peuvent maintenant se connecter au serveur d'application, et vous n'avez pas besoin de maintenir une liste d'adresses IP sources approuvées dans les paramètres du groupe de sécurité.

### Tâche 6 : Restriction de l'accès HTTP en utilisant une ACL réseau

Les groupes de sécurité ne sont pas la seule méthode que vous pouvez utiliser pour sécuriser l'accès aux ports réseau. Dans cette tâche, vous explorerez comment utiliser les ACL réseau pour contrôler l'accès aux instances EC2 dans un VPC.

1. Ajoutez une nouvelle règle entrante sur l'ACL réseau du LabVPC pour refuser tout le trafic entrant sur le port 80.
    - Retournez à l'onglet du navigateur où la console Amazon VPC est ouverte.
    - Dans le panneau de navigation, choisissez **Network ACLs** (ACL réseau).
    - Sélectionnez l'ACL réseau associée au LabVPC.
    - Choisissez l'onglet **Inbound rules** (Règles entrantes), puis choisissez **Edit inbound rules** (Modifier les règles entrantes).
    - Choisissez **Add new rule** (Ajouter une nouvelle règle), et configurez comme suit :
        - Rule number : Entrez **99**.
        - Type : Choisissez **HTTP**.
        - Allow/Deny : Choisissez **Deny** (Refuser).
    - Choisissez **Save changes** (Sauvegarder les modifications).

2. Testez l'accès au site web depuis ProxyServer1.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer1PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Après quelques instants, la connexion expire.
    - Note : Si le site web se charge,

 effectuez un rafraîchissement complet de l'onglet du navigateur.
    - Analyse : La règle de l'ACL réseau que vous avez créée bloque toutes les tentatives de connexion sur le port 80 vers toute instance EC2 fonctionnant dans le VPC. Le groupe de sécurité ProxySG autorise le trafic entrant sur le port 80, mais la connexion échoue car l'ACL réseau et le groupe de sécurité doivent tous deux l'autoriser.
    - Fermez l'onglet où vous avez tenté de charger le site web du serveur d'application.

3. Ajoutez une autre règle à l'ACL réseau.
    - Dans la console Amazon VPC, avec l'ACL réseau LabVPC toujours sélectionnée, choisissez **Edit inbound rules** (Modifier les règles entrantes).
    - Choisissez **Add new rule** (Ajouter une nouvelle règle) et configurez comme suit :
        - Rule number : Entrez **98**.
        - Type : Choisissez **HTTP**.
        - Allow/Deny : Choisissez **Allow** (Autoriser).
    - Choisissez **Save changes** (Sauvegarder les modifications).

4. Testez à nouveau l'accès au site web depuis ProxyServer1.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer1PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Le site web se charge.
    - Analyse : Toutes les règles définies dans une ACL réseau ont un numéro de règle. Les règles sont évaluées dans l'ordre, en commençant par la règle la plus basse. Lorsqu'une règle correspond au trafic, elle est appliquée indépendamment de toute règle numérotée plus élevée qui pourrait la contredire.
    - Fermez l'onglet où vous avez chargé le site web du serveur d'application.

### Tâche 7 : Connexion à l'AppServer en utilisant un bastion host et SSH

Dans cette tâche suivante, vous commencerez à explorer comment vous pouvez vous connecter au terminal de l'AppServer. Dans cette tâche, vous utiliserez une clé SSH pour vous connecter à un serveur dans PublicSubnetB en utilisant SSH sur le port TCP 22. Ensuite, en utilisant le transfert de port SSH, vous pourrez vous connecter à l'AppServer, qui fonctionne dans le sous-réseau privé.

1. Renommez l'instance EC2 ProxyServer2.
    - Dans la console Amazon EC2, choisissez **Instances**, puis sélectionnez l'instance ProxyServer2.
    - Dans la colonne **Name** (Nom), placez votre curseur sur ProxyServer2, puis choisissez l'icône d'édition qui apparaît.
    - Renommez le serveur en **Bastion** et choisissez **Save** (Sauvegarder).
    - Note : Un bastion host (également appelé serveur de saut) est un serveur qui fournit un accès à un réseau privé à partir d'un réseau externe, tel qu'Internet. Dans un scénario réel, vous lanceriez une nouvelle instance EC2 renforcée en matière de sécurité pour agir en tant que bastion host. Cependant, pour des raisons de démonstration dans ce laboratoire, vous allez réutiliser ProxyServer2 pour devenir un bastion host.

2. Créez un nouveau groupe de sécurité avec le port 22 ouvert pour le bastion host.
    - Dans le panneau de navigation, choisissez **Security Groups** (Groupes de sécurité).
    - Choisissez **Create security group** (Créer un groupe de sécurité), et configurez comme suit :
        - Security group name : Entrez **BastionSG**.
        - Description : Entrez **BastionSG**.
        - VPC : Supprimez le VPC actuellement sélectionné en choisissant l'icône **X**. Ensuite, choisissez **LabVPC**.
        - Dans la section **Inbound rules** (Règles entrantes), choisissez **Add rule** (Ajouter une règle), et configurez comme suit :
            - Type : Choisissez **SSH**.
            - Source : Choisissez **Anywhere-IPv4** (Partout-IPv4).
        - En bas de la page, choisissez **Create security group** (Créer un groupe de sécurité).

3. Mettez à jour les paramètres de l'instance bastion pour utiliser le groupe de sécurité BastionSG.
    - Dans le panneau de navigation, choisissez **Instances**.
    - Sélectionnez l'instance Bastion.
    - Choisissez **Actions > Security > Change security groups** (Actions > Sécurité > Modifier les groupes de sécurité).
    - Supprimez le groupe de sécurité ProxySG.
    - Dans la boîte de recherche, recherchez et choisissez le groupe de sécurité BastionSG.
    - Choisissez **Add security group** (Ajouter un groupe de sécurité), puis choisissez **Save** (Sauvegarder).

4. Modifiez les règles entrantes pour le groupe de sécurité AppServerSG pour autoriser le trafic depuis le bastion host.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, et copiez la valeur BastionPrivateIP.
    - Dans la console Amazon EC2, dans le panneau de navigation, choisissez **Security Groups** (Groupes de sécurité).
    - Sélectionnez le groupe de sécurité AppServerSG.
    - Choisissez l'onglet **Inbound rules** (Règles entrantes), puis choisissez **Edit inbound rules** (Modifier les règles entrantes).
    - Choisissez **Add rule** (Ajouter une règle), et configurez comme suit :
        - Type : Choisissez **SSH**.
        - Source : Choisissez **Custom** (Personnalisé).
        - Ensuite, collez la valeur BastionPrivateIP des instructions du laboratoire dans le champ **Source**.
        - Après avoir collé, ajoutez **/32** à la fin de l'adresse IP.
    - Choisissez **Save rules** (Sauvegarder les règles).
    - Vous avez maintenant mis à jour le groupe de sécurité attaché au serveur d'application pour qu'il autorise les connexions SSH sur le port 22 à partir du bastion host.

5. Connectez-vous au bastion host en utilisant SSH.
    - Dans le panneau de terminal à côté de ces instructions, exécutez les deux commandes suivantes.
        - `exec ssh-agent bash`
        - `ssh-add ~/.ssh/labsuser.pem`
    - Note : La première commande active l'outil ssh-agent sur cette machine Ubuntu, qui existe en dehors du compte AWS du laboratoire. La seconde commande ajoute la clé SSH à l'agent. Le bastion host et l'AppServer accepteront la clé SSH labsuser.pem pour les demandes de connexion SSH.
    - Ensuite, pour vous connecter à l'instance bastion, exécutez la commande suivante. Remplacez `<BastionPublicIP>` par la valeur BastionPublicIP, que vous pouvez trouver en choisissant le lien **AWS Details** au-dessus de ces instructions.
        - `ssh -i ~/.ssh/labsuser.pem -A ec2-user@<BastionPublicIP>`
    - Lorsque vous êtes invité à confirmer si vous voulez vous connecter, entrez **yes**.
    - Astuce : Remarquez que l'invite de commande indique maintenant `[ec2-user@bastion]`, ce qui signifie que vous êtes maintenant connecté à l'instance bastion.

6. Connectez-vous à l'AppServer via SSH en transférant la connexion authentifiée.
    - Exécutez la commande suivante. Remplacez `<AppServerPrivateIP>` par la valeur AppServerPrivateIP, que vous pouvez trouver en choisissant le lien **AWS Details** au-dessus de ces instructions.
        - `ssh ec2-user@<AppServerPrivateIP>`
    - Lorsque vous êtes invité à confirmer si vous voulez vous connecter, entrez **yes**.
    - Vous êtes maintenant connecté à l'AppServer.
    - Astuce : Remarquez que l'invite de commande indique maintenant `[ec2-user@appserver]`, ce qui signifie que vous êtes maintenant connecté à l'instance AppServer.
    - Pour créer un fichier pour prouver que vous vous êtes connecté à l'instance AppServer, exécutez la commande suivante. (La présence de ce fichier sera vérifiée lorsque vous soumettrez le laboratoire.)
        - `touch newfile.txt`
    - Analyse détaillée : Vous avez pu vous connecter à un serveur dans un sous-réseau privé car, lorsque vous avez émis la commande ssh depuis le serveur Ubuntu en dehors du compte, vous avez inclus `-A` dans la commande. Cela permet de transférer la connexion de l'agent authentifié.
    - La clé SSH labsuser.pem n'est pas stockée sur le bastion host, mais la session SSH que vous avez ouverte avec succès depuis le serveur extérieur vers le bastion host l'a incluse. Les paramètres SSH de l'AppServer sont configurés pour accepter la même clé SSH labsuser.pem.
    - De plus, la connexion à l'AppServer a été effectuée via le bastion host, que vous avez configuré dans les règles entrantes du groupe de sécurité AppServerSG comme une source acceptable pour les connexions SSH. Par conséquent, la connexion a réussi. Il est important de noter qu'en utilisant cette approche, vous avez évité de stocker la clé SSH sur le bastion host, ce qui n'est pas une bonne pratique de sécurité.
    - Le diagramme suivant montre la configuration que vous venez de configurer et d'utiliser.

7. Dans le terminal, pour vous déconnecter de l'AppServer, exécutez la commande `exit`.
    - Astuce : Remarquez que l'invite de commande indique maintenant `[ec2-user@bastion]

`, ce qui signifie que vous avez interrompu la connexion à l'AppServer et que vous êtes de nouveau sur l'instance bastion.

8. Pour vous déconnecter du serveur bastion, exécutez la commande `exit`.
    - Remarquez que l'invite indique maintenant que vous êtes connecté au terminal Ubuntu (comme indiqué par @runweb dans l'invite). Le serveur Ubuntu existe en dehors du compte AWS.

### Tâche 8 : Connexion directe à un hôte dans un sous-réseau privé en utilisant Session Manager

Que devriez-vous faire si vous devez modifier les paramètres de configuration de votre serveur d'application ? Vous devriez vous connecter à l'un des serveurs dans un sous-réseau public (souvent appelé bastion host ou jump box) et utiliser cela pour vous connecter au serveur dans le sous-réseau privé.

Parce que vos instances sont situées dans le cloud AWS, vous pouvez utiliser AWS Systems Manager Session Manager pour vous connecter directement à une instance sans ouvrir le port 22 pour SSH ou vous connecter via un autre serveur dans un sous-réseau public d'abord. Pour que cela fonctionne, l'agent Session Manager doit être installé sur l'hôte cible. Les instances Amazon Linux EC2 ont déjà l'agent Session Manager installé. Vous pouvez vous connecter avec Session Manager en utilisant une URL via un navigateur, comme vous l'avez fait dans ce laboratoire, ou via l'interface de ligne de commande AWS (AWS CLI).

Utiliser un bastion host est une pratique recommandée dans un réseau VPC, mais utiliser Session Manager peut être une alternative à la configuration et à la maintenance d'un bastion host. En utilisant Session Manager, AWS effectue le travail lourd de sécurisation de la connexion et de gestion d'une configuration de bastion host sécurisée et redondante. Cela vous donne également la possibilité d'utiliser AWS Identity and Access Management (IAM) pour accorder l'accès aux utilisateurs et crée une piste de journalisation auditable lorsqu'il est intégré avec AWS CloudTrail et Amazon CloudWatch.

Dans cette tâche, vous vous connecterez directement à l'instance d'application, qui n'est pas accessible sur Internet. Vous utiliserez Session Manager, comme montré dans le diagramme suivant.

1. Connectez-vous à l'AppServer en utilisant Session Manager.
    - Dans l'onglet du navigateur où la console Amazon EC2 est ouverte, choisissez **Instances**.
    - Sélectionnez l'AppServer, et choisissez **Connect** dans le coin supérieur droit.
    - Choisissez l'onglet **Session Manager**, puis choisissez **Connect**.
    - Un nouvel onglet du navigateur s'ouvre, et vous êtes connecté au terminal de l'AppServer avec une invite sh-4.2.
    - Vous êtes maintenant connecté à une session SSH sur le serveur d'application sans avoir à utiliser un bastion host. Rappelez-vous également que la règle de l'ACL réseau bloquant le trafic sur le port 22 est toujours configurée.

2. Pour modifier le site web hébergé sur ce serveur, exécutez la commande suivante :
    - `sudo sed -i 's/instance!/instance! Session manager was used to edit this file./g' /var/www/html/index.html`

3. Testez à nouveau l'accès au site web hébergé sur l'AppServer, en utilisant l'adresse IP publique de ProxyServer1.
    - Choisissez le lien **AWS Details** au-dessus de ces instructions, copiez la valeur ProxyServer1PublicIP, puis collez-la dans un nouvel onglet du navigateur.
    - Le site web du serveur d'application se charge. Remarquez que le site web du serveur d'application indique maintenant que Session Manager a été utilisé pour modifier ce fichier.
    - Note : Si nécessaire, effectuez un rafraîchissement complet de l'onglet du navigateur pour vous assurer que le site web ne se charge pas depuis le cache local.

### Soumission de votre travail

1. Pour enregistrer votre progression, choisissez **Submit** en haut de ces instructions.
    - Lorsque vous êtes invité, choisissez **Yes**.
    - Après quelques minutes, le panneau de notes apparaît et montre combien de points vous avez obtenus pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez **Grades** en haut de ces instructions.
    - Astuce : Vous pouvez soumettre votre travail plusieurs fois. Après avoir modifié votre travail, choisissez **Submit** à nouveau. Votre dernière soumission est enregistrée pour ce laboratoire.

2. Pour trouver des commentaires détaillés sur votre travail, choisissez **Submission Report**.

### Laboratoire terminé

Félicitations ! Vous avez terminé le laboratoire.

1. En haut de cette page, choisissez **End Lab**, puis choisissez **Yes** pour confirmer que vous souhaitez terminer le laboratoire.
    - Un panneau de message indique que le laboratoire est en train de se terminer.
2. Pour fermer le panneau, choisissez **Close** dans le coin supérieur droit.
