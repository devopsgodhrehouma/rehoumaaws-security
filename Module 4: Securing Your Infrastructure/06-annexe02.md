# Vulgarisation du Lab: Création d'un Environnement VPC

# Introduction au Lab

Dans ce lab, nous allons créer un environnement de réseau sur AWS, appelé VPC (Virtual Private Cloud), qui permettra de sécuriser et gérer les ressources de votre café. Nous utiliserons des sous-réseaux publics et privés, un hôte bastion pour l'accès administratif, et une passerelle NAT pour l'accès Internet depuis les sous-réseaux privés.

### Tâche 1 : Création d'un Sous-réseau Public

**Analogie : Jardin Public**
- **Action** : Vous créez une partie de votre terrain (VPC) accessible à tous.
- **Explication** : Cela correspond à la création d'un sous-réseau public dans le VPC où les ressources peuvent communiquer avec Internet.

### Tâche 2 : Création d'un Hôte Bastion

**Analogie : Gardien à la Porte d'Entrée**
- **Action** : Vous engagez un gardien pour surveiller l'accès à votre jardin public.
- **Explication** : L'hôte bastion est une instance EC2 dans le sous-réseau public qui permet une connexion sécurisée aux autres instances.

### Tâche 3 : Attribution d'une Adresse IP Elastic à l'Hôte Bastion

**Analogie : Donner une Clé Spéciale au Gardien**
- **Action** : Vous donnez au gardien une clé spéciale pour ouvrir la porte.
- **Explication** : Une adresse IP Elastic permet à l'hôte bastion d'être accessible depuis Internet.

### Tâche 4 : Tester la Connexion à l'Hôte Bastion

**Analogie : Tester la Porte d'Entrée**
- **Action** : Vous vérifiez que la porte s'ouvre correctement avec la clé spéciale.
- **Explication** : Vous utilisez SSH pour tester la connexion à l'hôte bastion.

### Tâche 5 : Création d'un Sous-réseau Privé

**Analogie : Cour Arrière Privée**
- **Action** : Vous créez une partie de votre terrain qui est privée et sécurisée.
- **Explication** : Un sous-réseau privé est une zone où les ressources ne sont pas accessibles directement depuis Internet.

### Tâche 6 : Création d'une Passerelle NAT

**Analogie : Petite Porte de Liaison**
- **Action** : Vous ajoutez une petite porte dans la clôture entre le jardin public et la cour arrière.
- **Explication** : Une passerelle NAT permet aux instances dans le sous-réseau privé d'accéder à Internet de manière sécurisée.

### Tâche 7 : Création d'une Instance EC2 dans le Sous-réseau Privé

**Analogie : Ajouter une Pièce Privée**
- **Action** : Vous ajoutez une pièce spéciale dans la cour arrière.
- **Explication** : Vous créez une instance EC2 dans le sous-réseau privé pour exécuter des tâches sensibles.

### Tâche 8 : Configuration du Client SSH pour le Relais SSH

**Analogie : Préparer votre Clé Personnelle**
- **Action** : Vous configurez votre clé pour accéder à la pièce privée depuis le jardin public.
- **Explication** : Vous configurez SSH pour utiliser différentes paires de clés pour accéder aux instances dans le sous-réseau privé.

### Tâche 9 : Test de la Connexion SSH de l'Hôte Bastion à l'Instance Privée

**Analogie : Tester la Sécurité**
- **Action** : Vous vérifiez que vous pouvez accéder à la pièce privée depuis le jardin public en passant par la cour arrière.
- **Explication** : Vous testez la connexion SSH depuis l'hôte bastion à l'instance privée et vérifiez la connectivité Internet.

### Tâche 10 : Création d'une ACL Réseau

**Analogie : Ajouter une Sécurité Supplémentaire**
- **Action** : Vous mettez en place des règles strictes pour contrôler qui peut entrer dans la cour arrière depuis le jardin public.
- **Explication** : Une ACL réseau contrôle le trafic entre les sous-réseaux pour une sécurité supplémentaire.

### Tâche 11 : Tester votre ACL Réseau

**Analogie : Vérifier les Règles de Sécurité**
- **Action** : Vous vérifiez que vos règles de sécurité fonctionnent en bloquant le trafic entre les zones publiques et privées.
- **Explication** : Vous créez une instance de test dans le sous-réseau public et modifiez l'ACL pour bloquer le trafic ICMP depuis le sous-réseau privé.

### Conclusion

En suivant ces étapes, vous aurez créé un environnement VPC sécurisé, avec des sous-réseaux publics et privés, un hôte bastion pour l'accès administratif, et des règles strictes de sécurité pour protéger vos ressources. C'est comme gérer un terrain de jeu privé avec des zones publiques et privées, des gardiens pour surveiller l'accès, et des règles pour assurer la sécurité et le bon fonctionnement.

-----
# Annexe : 

## Annexe : Configuration d'un Environnement Amazon Virtual Private Cloud (VPC) pour Renforcer la Sécurité des Ressources dans un Café

### Résumé des Principales Étapes et Tâches

**Tâche 1 : Création d'un Sous-réseau Public dans le VPC d'Atelier, Création d'une Passerelle Internet, et Configuration de la Table de Routage pour Acheminer le Trafic vers la Passerelle Internet**

1. **Créer un Espace Public**
   - Imaginez que votre jardin avant est une zone publique. Tout le monde peut y accéder.

**Tâche 2 : Création d'un Hôte Bastion dans le Sous-réseau Public avec des Règles de Sécurité pour Permettre Uniquement le Trafic SSH Depuis Votre Adresse IP**

2. **Ajouter un Gardien**
   - Vous décidez d'ajouter un gardien à la porte d'entrée de votre jardin. C'est comme un portier à l'entrée du café. Seules les personnes de confiance peuvent passer.

**Tâche 3 : Attribution d'une Adresse IP Elastic à l'Hôte Bastion pour un Accès Depuis Internet**

3. **Donner une Clé Spéciale au Gardien**
   - Vous donnez au gardien une clé spéciale pour qu'il puisse ouvrir la porte d'entrée. Cette clé spéciale ne peut être utilisée que par le gardien.

**Tâche 4 : Test de la Connexion SSH à l'Hôte Bastion à l'Aide de la Clé SSH Fournie**

4. **Tester la Porte d'Entrée**
   - Vous vérifiez si la porte d'entrée s'ouvre lorsque le gardien l'utilise avec sa clé spéciale.

**Tâche 5 : Création d'un Sous-réseau Privé dans le VPC d'Atelier**

5. **Créer un Espace Privé**
   - Vous avez une cour arrière qui est privée, seules quelques personnes sont autorisées à y entrer.

**Tâche 6 : Création d'une Passerelle NAT pour Permettre aux Ressources dans le Sous-réseau Privé d'Accéder à Internet**

6. **Ajouter une Porte de Liaison**
   - Vous ajoutez une porte spéciale dans la clôture entre le jardin avant (public) et la cour arrière (privée). Cette porte spéciale permet aux gens de la cour arrière d'aller dans le jardin avant, mais pas l'inverse.

**Tâche 7 : Création d'une Instance EC2 dans le Sous-réseau Privé avec des Règles de Sécurité pour Permettre Uniquement le Trafic SSH Depuis l'Hôte Bastion**

7. **Ajouter une Pièce Privée**
   - Dans la cour arrière, vous avez une pièce spéciale où seuls quelques amis proches sont autorisés à entrer.

**Tâche 8 : Configuration du Client SSH pour le Relais SSH Afin d'Utiliser une Paire de Clés Différente pour l'Accès à l'Instance Privée**

8. **Préparer votre Clé Personnelle**
   - Vous avez une clé spéciale que vous utilisez pour accéder à la pièce privée de la cour arrière.

**Tâche 9 : Test de la Connexion SSH Depuis l'Hôte Bastion vers l'Instance Privée et Vérification de la Connectivité Internet Depuis l'Instance Privée**

9. **Tester la Sécurité**
   - Vous vérifiez que vous pouvez utiliser votre clé spéciale pour accéder à la pièce privée depuis le jardin avant en passant par la cour arrière.

**Tâche 10 : Création d'une ACL Réseau Personnalisée pour Contrôler le Trafic Entre les Sous-réseaux Public et Privé**

10. **Ajouter une Sécurité Supplémentaire**
    - Vous mettez en place des règles strictes pour contrôler qui peut entrer dans la cour arrière depuis le jardin avant, comme un garde de sécurité à la porte.

**Tâche 11 : Création d'une Instance EC2 de Test dans le Sous-réseau Public et Test du Blocage du Trafic ICMP Depuis le Sous-réseau Privé vers l'Instance de Test en Modifiant la Liste ACL Réseau Personnalisée**

11. **Vérifier les Règles de Sécurité**
    - Vous vérifiez que vos règles de sécurité fonctionnent en bloquant l'accès depuis la cour arrière vers une zone de test.

### Conclusion

Après avoir suivi ces étapes, vous aurez configuré un environnement VPC avec des couches de sécurité pour protéger vos ressources. Cela permettra un accès administratif sécurisé à l'instance EC2 dans le sous-réseau privé via l'hôte bastion, tout en contrôlant le trafic entre les sous-réseaux avec l'ACL réseau personnalisée.

----
# Références : 
## Lab 4 -réseaux
- https://drive.google.com/drive/folders/1tqaa6CXHFPMbTkQEKEKRw2VoRnvDLypn?usp=sharing
