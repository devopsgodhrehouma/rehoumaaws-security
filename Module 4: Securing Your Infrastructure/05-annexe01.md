## Tutorial sur les CIDR et Exercices d'adresses IP

### 1. Introduction au CIDR (Classless Inter-Domain Routing)

CIDR est une méthode de notation des adresses IP et des routes qui remplace l'ancienne méthode basée sur les classes A, B, et C. L'objectif principal du CIDR est de permettre une allocation plus flexible et efficace des adresses IP.

### 2. Notation CIDR

Une adresse IP avec CIDR est notée de la forme `x.x.x.x/y` où :
- `x.x.x.x` est l'adresse IP
- `y` est le nombre de bits utilisés pour le préfixe réseau.

Par exemple, `192.168.1.0/24` signifie que les 24 premiers bits de l'adresse IP sont utilisés pour identifier le réseau, et les 8 bits restants sont utilisés pour identifier les hôtes dans ce réseau.

### 3. Exercice d'adresses IP

#### (1) Adresse IP `x.x.x.x/24`

- **Partie réseau** : Les 24 premiers bits (en jaune)
- **Partie hôtes** : Les 8 bits restants (en vert)
- **Calcul des hôtes** : 256 adresses - 3 adresses réservées (adresse réseau, adresse de diffusion, passerelle par défaut) = 253 adresses disponibles

#### (2) Adresse IP `x.x.x.x/16`

- **Partie réseau** : Les 16 premiers bits (en jaune)
- **Partie hôtes** : Les 16 bits restants (en vert)
- **Calcul des hôtes** : 256 * 256 - 3 = 65,533 adresses disponibles

#### (3) Adresse IP `x.x.x.x/8`

- **Partie réseau** : Les 8 premiers bits (en jaune)
- **Partie hôtes** : Les 24 bits restants (en vert)
- **Calcul des hôtes** : 256 * 256 * 256 - 3 = 16,777,213 adresses disponibles

### 4. Nombre de bits pour les hôtes et le masque de sous-réseau

Pour une adresse IP `x.x.x.x/8` :
- **Bits pour le réseau** : 8 bits (11111111)
- **Bits pour les hôtes** : 24 bits (00000000.00000000.00000000)
- **Masque de sous-réseau** : 255.0.0.0

### 5. Calcul des hôtes et des réseaux valides

- **Nombre d'hôtes dans un réseau** : \(2^{n} - 2\), où `n` est le nombre de bits pour les hôtes.
- **Nombre de réseaux valides** : Dépend du nombre de bits alloués pour le réseau.

### 6. Plage d'adresses valides

Pour une adresse IP `1.0.0.0/8` :
- **Adresse réseau** : 1.0.0.0
- **Première adresse IP valide** : 1.0.0.1
- **Dernière adresse IP valide** : 1.255.255.254
- **Adresse de diffusion** : 1.255.255.255

### 7. Adresses IP privées et publiques

- **Adresses IP privées** : Utilisées uniquement dans des réseaux locaux (LAN), non routables sur Internet. Réservées par IANA.
- **Adresses IP publiques** : Assignées par les ISP (Internet Service Providers), routables sur Internet.

### 8. Plages d'adresses privées par classe

- **Classe A** : 10.0.0.0 - 10.255.255.255
- **Classe B** : 128.0.0.0 - 191.255.255.255
- **Classe C** : 192.168.0.0 - 192.168.255.255

### Exemples et Analogies

Pour mieux comprendre, nous utiliserons une analogie : **un terrain de jeu**.

#### Terrain de jeu

1. **Création d'une VPC** : Imaginez que vous avez un terrain de jeu privé dans une grande plaine (AWS). Vous mettez une clôture autour de votre terrain pour le sécuriser et une porte pour que certains amis puissent entrer.
2. **Création de sous-réseaux** : À l'intérieur de votre terrain, vous créez des zones pour différents jeux (sous-réseaux) - une pour le football, une autre pour le basket-ball, etc.
3. **Groupe de sécurité VPC** : Vous établissez des règles pour qui peut entrer et utiliser les équipements du terrain de jeu (instances dans la VPC).
4. **Lancement d'une instance de serveur Web** : Vous installez une petite cabane (instance EC2) sur votre terrain, où vos amis peuvent consulter vos collections de timbres (site Web).

#### Exemple spécifique

- **Sous-réseau public** : Jardin accessible à tous
- **Sous-réseau privé** : Cour arrière où seuls quelques amis proches peuvent entrer
- **Passerelle NAT** : Petite porte dans la clôture entre le jardin et la cour arrière, permettant aux gens de la cour arrière d'aller dans le jardin, mais pas l'inverse
- **Hôte bastion** : Gardien à la porte d'entrée du jardin, vérifiant l'identité avant d'autoriser l'accès à la cour arrière.

En suivant ces étapes et analogies, vous comprendrez mieux la configuration et l'utilisation des adresses IP et des sous-réseaux dans un environnement réseau.
