# Partie 1 - obligatoire

### Question de Discussion 1: Comparaison des Méthodes de Chiffrement
**Consigne :**
Discutez des avantages et des inconvénients du chiffrement côté client comparé au chiffrement côté serveur avec AWS KMS. Comment ces méthodes affectent-elles la performance, la sécurité et la conformité? Quels scénarios d'usage spécifiques pourraient favoriser l'une par rapport à l'autre?

### Question de Discussion 2: Gestion des Clés dans AWS KMS
**Consigne :**
En groupes, explorez les meilleures pratiques pour la gestion du cycle de vie des clés de chiffrement dans AWS KMS. Discutez de comment implémenter efficacement la rotation des clés, la désactivation, et la suppression des clés, et les implications de ces actions sur la sécurité des données. Quels outils AWS peuvent aider dans ce processus et comment?

### Question de Discussion 3: Implications de la Sécurité Multicouche
**Consigne :**
Analysez l'importance d'une approche de sécurité multicouche dans le cloud. Comment AWS KMS s'intègre-t-il dans une stratégie de défense en profondeur? Discutez de différents niveaux de sécurité qui peuvent être renforcés par l'utilisation de AWS KMS en conjonction avec d'autres services AWS.

### Question de Discussion 4: Scénarios de Panne et Récupération
**Consigne :**
Évaluez les défis de la récupération des données chiffrées en cas de panne de la gestion des clés. Quelles stratégies pouvez-vous envisager pour récupérer les données si l'accès à une clé AWS KMS est perdu ou si la clé est corrompue? Comment la conception de la redondance et des sauvegardes affecte-t-elle ces stratégies?

### Question de Discussion 5: AWS KMS et Conformité Réglementaire
**Consigne :**
Discutez de la manière dont AWS KMS peut aider une organisation à répondre aux exigences de conformité réglementaire pour le chiffrement des données. Quels sont les défis associés à l'utilisation d'AWS KMS pour la conformité dans différents secteurs (financier, santé, éducation)? Comment les audits et les rapports facilités par AWS CloudTrail et d'autres services AWS soutiennent-ils ces efforts?

---
# Partie 2 - avancée (optionnelle)


### Question de Développement 1: Analyse de Cas d'Utilisation
**Consigne :**  
Imaginez que vous êtes un architecte de solutions chez une entreprise qui doit se conformer à des réglementations strictes en matière de protection des données personnelles. Décrivez comment vous utiliseriez AWS KMS pour garantir la sécurité des données au repos sur les services AWS suivants : Amazon S3, Amazon EBS et Amazon RDS. Incluez les étapes spécifiques pour configurer le chiffrement, les types de clés à utiliser, et comment vous géreriez le cycle de vie des clés. Discutez également des implications de la conformité réglementaire.

### Question de Développement 2: Conception d'une Solution de Sécurité
**Consigne :**  
Développez un plan détaillé pour mettre en place une stratégie de chiffrement des données en utilisant AWS KMS dans une architecture multi-couche, comprenant des applications web, des bases de données et du stockage de fichiers. Votre plan doit inclure la sélection du type de clé (clés gérées par AWS vs clés gérées par le client), la gestion des permissions, la rotation des clés, et l'intégration avec AWS IAM pour contrôler l'accès. Expliquez comment cette stratégie améliore la sécurité globale et répond aux exigences de conformité spécifiques.

### Question de Développement 3: Évaluation et Gestion des Risques
**Consigne :**  
Écrivez un rapport sur les risques associés à l'utilisation d'AWS KMS pour le chiffrement des données sensibles dans un environnement cloud. Analysez les risques potentiels tels que la compromission de clés, l'accès non autorisé, et la perte de données. Proposez des mesures de mitigation pour chaque risque identifié, y compris des techniques de sécurité en couches, des audits réguliers, et l'utilisation de politiques de sécurité strictes. Discutez de la manière dont AWS CloudTrail peut être utilisé pour surveiller et auditer l'utilisation des clés.

### Question de Développement 4: Défi Technique et Solution
**Consigne :**  
Supposez que vous devez récupérer des données chiffrées d'un snapshot EBS qui utilisait une clé AWS KMS désormais désactivée. Décrivez étape par étape comment vous procéderiez pour récupérer ces données, y compris les interactions nécessaires avec AWS KMS pour réactiver la clé ou en créer une nouvelle compatible. Examinez les implications techniques de la réactivation des clés et les meilleures pratiques pour éviter de telles situations à l'avenir.

