# Schéma 1 
![image](https://github.com/user-attachments/assets/4f5d941a-3aa9-487a-bf1e-9fed1861114d)


# Schéma 1 : Infrastructure de Réseau avec Groupes de Sécurité Proxy et Serveur d'Application

#### Explication pour Débutant :

**Description du Schéma :**

Le schéma montre une infrastructure réseau dans AWS avec plusieurs composants :

- **VPC (Virtual Private Cloud) :** C'est une section isolée du cloud AWS où vous pouvez lancer des ressources AWS.
- **Subnets Publics et Privés :** Les subnets publics peuvent communiquer avec l'internet, tandis que les subnets privés ne le peuvent pas directement.
- **Instances EC2 (Serveurs) :** Il y a deux instances proxy dans les subnets publics et une instance d'application dans le subnet privé.
- **Route Tables (Tables de Routage) :** Définissent comment le trafic doit être routé dans le VPC.

**Détails du Schéma :**

1. **LabVPC (10.0.0.0/16) :** Un VPC avec une plage d'adresses IP.
2. **PublicSubnetA (10.0.1.0/24) :** Contient ProxyServer1.
3. **PublicSubnetB (10.0.2.0/24) :** Contient ProxyServer2.
4. **PrivateSubnet (10.0.11.0/24) :** Contient AppServer.
5. **Internet Gateway :** Permet aux instances dans les subnets publics de se connecter à l'internet.
6. **NAT Gateway :** Permet aux instances dans les subnets privés de se connecter à l'internet de manière sécurisée.
7. **Security Groups (SG) :** Agissent comme des pare-feu virtuels pour contrôler le trafic entrant et sortant des instances EC2.

**Fonctionnement :**

- **ProxyServer1 et ProxyServer2** dans les subnets publics sont configurés pour acheminer le trafic vers **AppServer** dans le subnet privé.
- Le **NAT Gateway** permet aux instances du subnet privé de se connecter à l'internet pour télécharger des mises à jour ou accéder à d'autres ressources, mais empêche les connexions entrantes non sollicitées de l'internet.
- Les **Security Groups** pour ProxyServer1 et ProxyServer2 permettent le trafic HTTP (port 80) de n'importe quelle source.
- **AppServerSG** permet également le trafic HTTP, mais de manière restreinte pour des raisons de sécurité.

**Vulgarisation en Vraie Vie :**

Imaginez que votre maison (VPC) a plusieurs pièces (subnets). Certaines pièces sont accessibles à tous, comme le salon (subnets publics), tandis que d'autres sont privées comme votre chambre (subnet privé). Vous avez des gardes (proxy servers) dans le salon qui vérifient les visiteurs et les amènent à votre chambre s'ils sont approuvés. Vous avez aussi des règles (security groups) qui définissent qui peut entrer dans chaque pièce et comment ils peuvent interagir avec les objets dans ces pièces.

--------



🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schéma 2
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/669eb566-77d7-4258-87d9-e46c0147e866)


# Schéma 2 : Accès à Internet pour Serveur d'Application

**Description du Schéma :**

Ce schéma montre une infrastructure similaire à Schéma 1 mais sans les instances proxy dans les subnets publics. Seul le NAT Gateway est utilisé pour permettre aux instances dans le subnet privé d'accéder à internet.

**Fonctionnement :**

- Les instances dans le subnet privé utilisent le **NAT Gateway** pour accéder à l'internet.
- Le trafic sortant du **AppServer** (dans le subnet privé) passe par le NAT Gateway dans **PublicSubnetA**.
- Le NAT Gateway masque les adresses IP des instances privées lorsqu'elles communiquent avec l'internet.

**Vulgarisation en Vraie Vie :**

C'est comme si vous avez un intermédiaire (NAT Gateway) dans le salon qui transmet vos messages au monde extérieur. Les réponses reviennent à cet intermédiaire, qui les transmet ensuite à vous dans votre chambre. Cela permet de garder votre chambre cachée des étrangers.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schéma 3
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/02d02704-ec9c-45e8-8101-88383fd91833)


# Schéma 3 : Configuration avec Proxy et NAT Gateway

**Description du Schéma :**

Ce schéma combine des éléments de Schéma 1 et Schéma 2, montrant les instances proxy dans les subnets publics et le NAT Gateway.

**Fonctionnement :**

- **ProxyServer1 et ProxyServer2** dans les subnets publics dirigent le trafic vers **AppServer** dans le subnet privé.
- Le **NAT Gateway** est utilisé pour le trafic sortant de **AppServer** vers l'internet.
- Les **Security Groups** sont configurés pour permettre les communications nécessaires.

**Vulgarisation en Vraie Vie :**

Vous avez des gardes (proxy servers) dans le salon pour vérifier et diriger les visiteurs vers votre chambre. Vous avez aussi un intermédiaire (NAT Gateway) pour envoyer et recevoir des messages de l'extérieur sans révéler directement votre chambre.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schéma 4
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/0f7b222d-c61b-498d-9a9d-cf0cd147ae4e)


# Schéma 4 : Erreur de Configuration avec Proxy

**Description du Schéma :**

Ce schéma montre une configuration incorrecte où le ProxyServer2 ne peut pas communiquer avec AppServer à cause d'une erreur de routage ou de configuration de sécurité.

**Fonctionnement :**

- Le schéma montre un X rouge indiquant une erreur de connexion entre ProxyServer2 et AppServer.
- Cela peut être dû à une mauvaise configuration des **Security Groups** ou des **Route Tables**.

**Vulgarisation en Vraie Vie :**

C'est comme si l'un des gardes (ProxyServer2) dans le salon n'est pas autorisé à entrer dans votre chambre à cause d'une règle incorrecte ou d'un problème de communication.


🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schéma 5
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/00ec5f69-3df9-49f1-88b6-87ed444d5fbd)


# Schéma 5 : Connexion Correcte des Proxys

**Description du Schéma :**

Ce schéma montre la configuration correcte où les deux proxys (ProxyServer1 et ProxyServer2) peuvent accéder à AppServer.

**Fonctionnement :**

- Les proxys dans les subnets publics sont correctement configurés pour acheminer le trafic vers AppServer.
- Les **Security Groups** et **Route Tables** sont correctement configurés.

**Vulgarisation en Vraie Vie :**

Les gardes dans le salon peuvent maintenant correctement guider les visiteurs vers votre chambre sans problèmes de communication ou de règles incorrectes.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schéma 6
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/9076d5ac-5472-473f-a011-5f100930547c)


# Schéma 6 : Utilisation d'un Bastion Host pour SSH

**Description du Schéma :**

Ce schéma montre l'utilisation d'un Bastion Host pour se connecter de manière sécurisée à AppServer dans le subnet privé.

**Fonctionnement :**

- **Bastion Host** est une instance dans le subnet public utilisée comme point de saut pour accéder à AppServer.
- Utilisation d'un **SSH Key** pour sécuriser la connexion.
- **Security Groups** sont configurés pour permettre l'accès SSH uniquement via Bastion Host.

**Vulgarisation en Vraie Vie :**

C'est comme si vous avez un garde dans le salon (Bastion Host) avec une clé spéciale. Pour entrer dans votre chambre, les visiteurs doivent d'abord parler à ce garde, qui les guide ensuite jusqu'à vous.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schéma 7
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/80c76d5a-9f49-4991-9256-449df64d0ad1)



# Schéma 7 : Utilisation de AWS Systems Manager Session Manager

**Description du Schéma :**

Ce schéma montre l'utilisation de AWS Systems Manager Session Manager pour se connecter directement à AppServer sans avoir besoin d'un Bastion Host.

**Fonctionnement :**

- **Session Manager** permet de se connecter directement à AppServer via une interface web sécurisée.
- Évite la nécessité d'ouvrir le port SSH (22) ou d'utiliser un Bastion Host.
- Utilise des rôles IAM pour gérer l'accès et intègre la journalisation via CloudTrail et CloudWatch.

**Vulgarisation en Vraie Vie :**

Imaginez que vous pouvez parler directement avec quelqu'un dans votre chambre via une application de messagerie sécurisée, sans avoir besoin de passer par le salon ou d'utiliser des clés physiques.






--------
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

# Schéma 1 
![image](https://github.com/user-attachments/assets/4f5d941a-3aa9-487a-bf1e-9fed1861114d)

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
--------
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schema 2
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/669eb566-77d7-4258-87d9-e46c0147e866)

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schema 3
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/02d02704-ec9c-45e8-8101-88383fd91833)

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Schema 4
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/0f7b222d-c61b-498d-9a9d-cf0cd147ae4e)

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Scehma 5
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/00ec5f69-3df9-49f1-88b6-87ed444d5fbd)

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Scehma 6
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/9076d5ac-5472-473f-a011-5f100930547c)

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
# Scehma 7
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

![image](https://github.com/user-attachments/assets/80c76d5a-9f49-4991-9256-449df64d0ad1)


