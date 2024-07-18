### Quiz sur le TP 5.1 : Chiffrement des données au repos avec AWS KMS

**Question 1**  
Quel service AWS utilisez-vous pour créer et gérer des clés de chiffrement pour sécuriser vos données ?  
A) AWS CloudTrail  
B) AWS KMS  
C) Amazon EC2  
D) Amazon S3

**Question 2**  
Quelle est la fonction principale de AWS KMS dans ce TP ?  
A) Monitorer le trafic réseau  
B) Gérer des instances EC2  
C) Créer et gérer des clés de chiffrement  
D) Stocker des données non chiffrées

**Question 3**  
Que permet de faire le journal d'audit AWS CloudTrail en relation avec AWS KMS ?  
A) Stocker des clés de chiffrement  
B) Enregistrer les accès et les utilisations des clés  
C) Augmenter la capacité de stockage des objets S3  
D) Crypter automatiquement les nouvelles instances EC2

**Question 4**  
Comment pouvez-vous chiffrer un objet dans Amazon S3 à l'aide de AWS KMS ?  
A) En activant l'option de chiffrement côté serveur SSE-KMS lors de l'upload de l'objet  
B) En créant un instantané du seau S3  
C) En configurant une instance EC2 pour utiliser AWS KMS  
D) En utilisant AWS CloudTrail pour activer le chiffrement

**Question 5**  
Quel est l'impact de la désactivation d'une clé AWS KMS utilisée pour chiffrer un volume EBS ?  
A) Le volume EBS est automatiquement supprimé  
B) L'instance EC2 utilisant ce volume ne pourra pas redémarrer  
C) Les données sur le volume EBS sont automatiquement déchiffrées  
D) Aucun impact, le volume continue de fonctionner normalement

**Question 6**  
Quelle affirmation décrit correctement une clé gérée par le client (CMK) dans AWS KMS ?  
A) Une CMK est toujours gérée et maintenue par AWS sans intervention de l'utilisateur  
B) Les utilisateurs ne peuvent pas créer ou gérer leurs propres CMK  
C) Une CMK peut être utilisée pour chiffrer et déchiffrer des données au repos  
D) Une CMK permet uniquement le chiffrement des données en transit

**Réponses**  
1. B) AWS KMS  
2. C) Créer et gérer des clés de chiffrement  
3. B) Enregistrer les accès et les utilisations des clés  
4. A) En activant l'option de chiffrement côté serveur SSE-KMS lors de l'upload de l'objet  
5. B) L'instance EC2 utilisant ce volume ne pourra pas redémarrer  
6. C) Une CMK peut être utilisée pour chiffrer et déchiffrer des données au repos

