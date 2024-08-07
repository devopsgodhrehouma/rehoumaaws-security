# Autoriser le trafic HTTP (port 80)

- Pour autoriser le trafic HTTP (port 80) dans un groupe de sécurité réseau (NSG) sur le portail Azure, suivez les étapes ci-dessous. 
- Je vais détailler les paramètres spécifiques que vous devrez configurer en utilisant l'interface suivante :

![image](https://github.com/user-attachments/assets/62138566-aa84-4b7b-bfe5-4e644fc29211)


# 01 - Étapes pour Autoriser le Trafic HTTP (Port 80) :

1. **Accéder au NSG :**
   - Allez sur le portail Azure et connectez-vous.
   - Dans le menu de gauche, sélectionnez "Tous les services" et recherchez "Groupes de sécurité réseau".
   - Sélectionnez votre NSG (par exemple, "HelloWorldVm-nsg").

2. **Ajouter une Nouvelle Règle de Sécurité Entrante :**
   - Dans la section des règles de sécurité entrantes, cliquez sur "Ajouter".

3. **Configurer la Règle de Sécurité Entrante :**
   - **Source :** Sélectionnez "Any" (N'importe quelle).
   - **Plages de ports sources :** Mettez `*` (pour tous les ports sources).
   - **Destination :** Sélectionnez "Any" (N'importe quelle).
   - **Service :** Choisissez "Custom" (Personnalisé).
   - **Plages de ports de destination :** Entrez `80` (le port HTTP).
   - **Protocole :** Sélectionnez "TCP" (ou "Any" si vous préférez).
   - **Action :** Sélectionnez "Autoriser".
   - **Priorité :** Définissez la priorité (par exemple, `310`). Assurez-vous que ce numéro est unique et ne cause pas de conflits avec d'autres règles.
   - **Nom :** Donnez un nom à la règle, par exemple "AllowHTTPInbound".

4. **Enregistrer la Règle :**
   - Cliquez sur "Ajouter" pour créer et enregistrer la règle.

Voici comment vous pouvez configurer les paramètres de la règle dans l'interface que vous avez fournie :

- **Source :** Any
- **Plages de ports sources :** *
- **Destination :** Any
- **Service :** Custom
- **Plages de ports de destination :** 80
- **Protocole :** TCP (ou Any)
- **Action :** Autoriser
- **Priorité :** 310 (ou un autre numéro de priorité)
- **Nom :** AllowHTTPInbound

# 02 - Exemple visuel des paramètres configurés pour autoriser le trafic HTTP :

![image](https://github.com/user-attachments/assets/55e3d434-cfe5-40b6-9ef9-55bc28405ddb)


En suivant ces étapes, vous allez créer une règle de sécurité entrante dans votre NSG qui permet le trafic HTTP (port 80) à partir de n'importe quelle source. Assurez-vous de bien définir les priorités pour éviter des conflits avec d'autres règles existantes.
