# Illustrer comment CloudTrail, CloudWatch, SNS et EventBridge fonctionnent ensemble dans un scénario de surveillance et d'alerte 

## Objectif :
- Illustrer comment CloudTrail, CloudWatch, SNS et EventBridge fonctionnent ensemble dans un scénario de surveillance et d'alerte :

```plaintext
                            +---------------------+
                            |  Utilisateur ou     |
                            |  Service AWS        |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |   Appel d'API       |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |     CloudTrail      |
                            | (Caméra de surveillance)|
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  Enregistrement     |
                            |  d'événement        |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  EventBridge        |
                            | (Centre de commande)|
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |   Règle EventBridge |
                            | (Détection de       |
                            |  modification)      |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  Détection et       |
                            |  Déclenchement      |
                            |  d'alarme           |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |     CloudWatch      |
                            | (Détecteur de       |
                            |  mouvement)         |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |    Alarme           |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |   SNS               |
                            | (Système d'alerte)  |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  Notification       |
                            |  par Email/SMS      |
                            +---------------------+

```

### Description du Schéma :

1. **Utilisateur ou Service AWS :** Effectue une action dans le compte AWS (par exemple, tente de se connecter ou modifie une règle de sécurité).
2. **Appel d'API :** L'action est enregistrée comme un appel d'API.
3. **CloudTrail (Caméra de surveillance) :** CloudTrail enregistre l'appel d'API comme un événement.
4. **EventBridge (Centre de commande) :** EventBridge détecte l'événement enregistré par CloudTrail et applique une règle pour déterminer la réaction appropriée.
5. **Règle EventBridge :** La règle EventBridge peut surveiller des événements spécifiques comme les modifications de groupes de sécurité.
6. **CloudWatch (Détecteur de mouvement) :** Si l'événement correspond à une condition critique (par exemple, plusieurs tentatives de connexion échouées), CloudWatch déclenche une alarme.
7. **SNS (Système d'alerte) :** L'alarme déclenchée par CloudWatch utilise SNS pour envoyer une notification par email ou SMS.
8. **Notification par Email/SMS :** Le responsable de la sécurité ou l'administrateur reçoit l'alerte en temps réel et peut prendre les mesures nécessaires.

- Ce schéma montre comment ces services AWS travaillent ensemble pour assurer une surveillance continue, détecter les événements critiques, et notifier les administrateurs en temps réel, renforçant ainsi la sécurité et la réactivité de votre environnement AWS.

---
# Vulgarisation

```plaintext
                            +---------------------+
                            |     Maison          |
                            | (Propriétaire)      |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |   Caméra de         |
                            |   surveillance      |
                            | (CloudTrail)        |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  Enregistrement     |
                            |  d'activité         |
                            | (CloudTrail)        |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |    Centre de        |
                            |    commande         |
                            | (EventBridge)       |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  Détection de       |
                            |  mouvement          |
                            | (CloudWatch)        |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |   Déclenchement     |
                            |   d'alarme          |
                            | (CloudWatch Alarme) |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |    Système          |
                            |    d'alerte         |
                            | (SNS)               |
                            +---------+-----------+
                                      |
                                      v
                            +---------------------+
                            |  Notification       |
                            |  au propriétaire    |
                            | (Email/SMS via SNS) |
                            +---------------------+

```

### Description du Schéma :

1. **Maison (Propriétaire) :**
   - La maison est équipée de systèmes de sécurité pour surveiller et protéger les lieux.

2. **Caméra de surveillance (CloudTrail) :**
   - La caméra surveille l'extérieur et l'intérieur de la maison et enregistre toutes les activités, tout comme CloudTrail enregistre toutes les actions effectuées dans le compte AWS.

3. **Enregistrement d'activité (CloudTrail) :**
   - La caméra enregistre toute activité détectée, similaire à la manière dont CloudTrail enregistre les appels d'API.

4. **Centre de commande (EventBridge) :**
   - Le centre de commande reçoit les enregistrements de la caméra et analyse les activités détectées, tout comme EventBridge analyse les événements enregistrés par CloudTrail.

5. **Détection de mouvement (CloudWatch) :**
   - Le système de détection de mouvement analyse les enregistrements pour identifier toute activité suspecte ou inhabituelle, similaire à CloudWatch qui surveille les métriques et les journaux pour détecter des anomalies.

6. **Déclenchement d'alarme (CloudWatch Alarme) :**
   - Si une activité suspecte est détectée, comme une personne entrant dans la maison sans autorisation, une alarme est déclenchée, tout comme CloudWatch déclenche une alarme en cas d'activité critique.

7. **Système d'alerte (SNS) :**
   - Le système d'alerte envoie une notification immédiate au propriétaire de la maison, tout comme SNS envoie des notifications par email ou SMS.

8. **Notification au propriétaire (Email/SMS via SNS) :**
   - Le propriétaire reçoit une alerte par SMS ou email l'informant de l'intrusion, similaire à la manière dont SNS envoie des notifications aux administrateurs.

### Exemple Réel : Maison Surveillée

1. **Étape 1 : Surveillance Continue (CloudTrail)**
   - Une caméra de surveillance est installée à l'entrée de la maison et enregistre toutes les activités.

2. **Étape 2 : Enregistrement de l'Activité (CloudTrail)**
   - La caméra enregistre une personne entrant dans la maison.

3. **Étape 3 : Analyse au Centre de Commande (EventBridge)**
   - Les enregistrements sont envoyés au centre de commande où ils sont analysés pour détecter toute activité suspecte.

4. **Étape 4 : Détection de Mouvement (CloudWatch)**
   - Le détecteur de mouvement identifie que quelqu'un est entré dans la maison en dehors des heures normales.

5. **Étape 5 : Déclenchement de l'Alarme (CloudWatch Alarme)**
   - Une alarme est déclenchée immédiatement pour avertir des occupants et dissuader l'intrus.

6. **Étape 6 : Envoi de la Notification (SNS)**
   - Le système d'alerte envoie une notification par SMS et email au propriétaire de la maison.

7. **Étape 7 : Réaction du Propriétaire (Email/SMS via SNS)**
   - Le propriétaire reçoit l'alerte en temps réel et peut appeler la police ou vérifier la situation via une application mobile connectée à la caméra.

### Comparaison avec AWS

- **Caméra de Surveillance (CloudTrail) :** Enregistre toutes les actions et événements se produisant dans le compte AWS.
- **Centre de Commande (EventBridge) :** Analyse les événements enregistrés et applique des règles pour déterminer la réponse appropriée.
- **Détection de Mouvement (CloudWatch) :** Surveille les activités en temps réel et déclenche des alarmes basées sur des seuils définis.
- **Système d'Alerte (SNS) :** Envoie des notifications immédiates par SMS ou email lorsque des événements critiques sont détectés.

En utilisant cette analogie, vous pouvez mieux comprendre comment les services AWS fonctionnent ensemble pour assurer une surveillance et une alerte efficaces, tout comme un système de sécurité sophistiqué protège votre maison.
