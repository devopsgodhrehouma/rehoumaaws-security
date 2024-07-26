Voici une table résumant les objectifs et les concepts liés à chaque tâche :

| Tâche  | Objectif                                                                                                                    | Concepts                                                                                         |
|--------|-----------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 1      | Examiner et comprendre les paramètres réseau et de routage du VPC et du sous-réseau privé.                                 | VPC (Virtual Private Cloud), sous-réseaux, tables de routage, NAT Gateway, CIDR.                 |
| 2      | Analyser les paramètres réseau, de routage et de sécurité des sous-réseaux publics du VPC.                                  | Sous-réseaux publics, tables de routage, Internet Gateway, groupes de sécurité.                  |
| 3      | Tester la connectivité à une page web hébergée sur l'AppServer depuis les instances EC2 publiques.                          | Groupes de sécurité, routage de sous-réseaux, connectivité HTTP, instances EC2.                  |
| 4      | Restreindre l'accès HTTP à l'AppServer à partir d'une seule adresse IP interne.                                             | Groupes de sécurité, filtrage par adresse IP, principes de moindre privilège.                    |
| 5      | Configurer le groupe de sécurité AppServerSG pour autoriser l'accès HTTP depuis les instances associées à un groupe de sécurité spécifique. | Référence de groupes de sécurité, gestion des accès, sécurité des réseaux.                       |
| 6      | Utiliser une ACL réseau pour contrôler et restreindre l'accès HTTP au VPC.                                                  | ACL réseau (Access Control List), règles de filtrage, priorités de règles, contrôle d'accès.     |
| 7      | Se connecter à l'AppServer en utilisant un bastion host et SSH pour sécuriser les connexions SSH.                           | Bastion host, SSH (Secure Shell), clés SSH, forwarding SSH, sécurité des connexions réseau.      |
| 8      | Se connecter directement à un serveur dans un sous-réseau privé en utilisant AWS Systems Manager Session Manager.          | AWS Systems Manager, Session Manager, accès sécurisé sans SSH, gestion des connexions.           |

### Explication des Concepts

1. **VPC (Virtual Private Cloud) et Sous-réseaux** :
   - **VPC** : Un réseau virtuel dédié dans le cloud AWS.
   - **Sous-réseaux** : Segments du VPC, pouvant être publics ou privés, pour isoler et gérer les ressources.

2. **Tables de Routage et Internet Gateway** :
   - **Tables de Routage** : Définissent comment le trafic est dirigé dans le VPC.
   - **Internet Gateway** : Permet la communication entre les instances du VPC et Internet.

3. **Groupes de Sécurité** :
   - **Groupes de Sécurité** : Pare-feu virtuels contrôlant le trafic entrant et sortant des instances EC2.

4. **NAT Gateway** :
   - **NAT Gateway** : Permet aux instances dans un sous-réseau privé d’accéder à Internet sans exposer leur adresse IP privée.

5. **Routage de Sous-réseaux et ACL Réseau** :
   - **Routage de Sous-réseaux** : Détermine les chemins que prend le trafic pour atteindre les différentes destinations.
   - **ACL Réseau** : Contrôlent le trafic entrant et sortant des sous-réseaux, évaluent les règles en fonction des numéros de règle.

6. **Bastion Host et SSH** :
   - **Bastion Host** : Serveur de saut sécurisé permettant d’accéder aux instances dans un sous-réseau privé.
   - **SSH** : Protocole sécurisé pour accéder et gérer les ressources à distance.

7. **AWS Systems Manager et Session Manager** :
   - **AWS Systems Manager** : Service qui aide à gérer les ressources AWS.
   - **Session Manager** : Permet une connexion sécurisée aux instances sans utiliser SSH, simplifiant la gestion et améliorant la sécurité.

Ces concepts sont essentiels pour comprendre et mettre en œuvre des stratégies de sécurité et de gestion des réseaux dans AWS.
