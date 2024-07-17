
![image](https://github.com/user-attachments/assets/7ffa1975-4fd8-47a5-aa83-6e0f62cecefd)
![image](https://github.com/user-attachments/assets/d33ac83f-2e8b-4963-9121-8c55d372353b)

### Explication simplifiée de l'architecture AWS Config et Lambda

#### Schéma et fonctionnement

**Schéma :**

Le schéma montre comment AWS Config et AWS Lambda travaillent ensemble pour surveiller et corriger les paramètres de sécurité des groupes de sécurité dans votre réseau AWS.

1. **Surveillance des modifications :**
   - AWS Config surveille constamment les groupes de sécurité pour toute modification.

2. **Détection de changement :**
   - Si AWS Config détecte un changement dans un groupe de sécurité, il appelle immédiatement AWS Lambda.

3. **Correction automatique :**
   - AWS Lambda ajuste automatiquement les paramètres du groupe de sécurité pour s'assurer qu'ils respectent les règles définies.

#### Étapes simplifiées :

1. **Surveillance :**
   - AWS Config garde un œil sur vos groupes de sécurité, un peu comme un gardien surveillant une maison.

2. **Détection :**
   - Si quelque chose change, AWS Config appelle AWS Lambda, comme un gardien qui alerterait la police en cas d'intrusion.

3. **Correction :**
   - AWS Lambda intervient pour corriger les paramètres, comme la police qui rétablit l'ordre.

### Résumé

En gros, cette architecture permet de s'assurer automatiquement que les paramètres de sécurité de vos groupes de sécurité sont toujours conformes aux règles définies, en surveillant et en corrigeant les changements de manière proactive.

#  Annexe 1: 

### Exemple de la vie réelle pour comprendre AWS Config et Lambda

Imaginons que vous gérez la sécurité d'une grande entreprise avec plusieurs bureaux.

#### Contexte :

1. **Surveillance continue :**
   - Vous avez des caméras de sécurité (AWS Config) dans chaque bureau qui surveillent les activités en permanence.

2. **Détection de changement :**
   - Les caméras sont programmées pour alerter immédiatement la sécurité (AWS Lambda) si elles détectent quelque chose de suspect, comme une porte laissée ouverte après les heures de travail.

3. **Intervention automatique :**
   - Une fois alertée, la sécurité (AWS Lambda) intervient immédiatement pour fermer la porte et s'assurer que tout est en ordre.

#### Étapes détaillées :

1. **Caméras de sécurité (AWS Config) :**
   - Ces caméras surveillent constamment les bureaux pour détecter toute anomalie ou changement.

2. **Alerte de sécurité (AWS Lambda) :**
   - Dès qu'une caméra détecte une porte laissée ouverte, elle envoie une alerte à l'équipe de sécurité.

3. **Intervention de la sécurité (AWS Lambda) :**
   - L'équipe de sécurité reçoit l'alerte et envoie un agent pour fermer la porte et s'assurer que le bureau est sécurisé.

### En résumé :

- **AWS Config** joue le rôle des caméras de sécurité, surveillant constamment les groupes de sécurité dans votre réseau AWS pour toute modification.
- **AWS Lambda** joue le rôle de l'équipe de sécurité, réagissant immédiatement à toute alerte pour corriger les problèmes et maintenir la sécurité.

Ainsi, cette architecture assure automatiquement que la sécurité de votre réseau est toujours maintenue, de la même manière que vous garantiriez la sécurité physique de vos bureaux avec des caméras et une équipe de sécurité.

#  Annexe 2:

### Explication de l'architecture AWS Config et Lambda

#### Diagramme et étapes de fonctionnement

**Diagramme :**

Le diagramme représente l'architecture d'AWS Config et son intégration avec AWS Lambda pour surveiller et appliquer des règles de sécurité sur des groupes de sécurité (Security Groups) au sein de différents VPC (Virtual Private Cloud).

1. **Règle AWS Config :**
   - La règle AWS Config surveille toute modification des groupes de sécurité répertoriés dans l'inventaire des ressources AWS Config.
   
2. **Fonction Lambda :**
   - Lorsqu'une modification est détectée sur un groupe de sécurité, la règle AWS Config invoque une fonction Lambda.
   
3. **Application de la règle :**
   - La fonction Lambda remédie à la situation en mettant à jour la configuration des règles d'entrée souhaitées pour le groupe de sécurité.

#### Explications détaillées :

1. **Étape 1 :**
   - La règle AWS Config surveille toute modification apportée aux groupes de sécurité suivis dans l'inventaire des ressources AWS Config.
   
2. **Étape 2 :**
   - Lorsque la règle détecte des modifications sur un groupe de sécurité, elle invoque la fonction Lambda.
   
3. **Étape 3 :**
   - La fonction Lambda remédie à la situation en mettant à jour la configuration des règles d'entrée souhaitées pour le groupe de sécurité.

### Synthèse

En résumé, cette architecture permet de surveiller automatiquement les groupes de sécurité dans AWS et de corriger toute modification non autorisée en temps réel en utilisant AWS Config et AWS Lambda.

- https://medium.com/@raggadaz/aws-config-ef084306cdcb
