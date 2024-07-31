# Annexe 03 : 

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
