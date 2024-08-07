Pour autoriser le trafic HTTP (port 80) dans un groupe de sécurité réseau (NSG) en utilisant Azure CLI, suivez les étapes ci-dessous :

### Étapes pour Autoriser le Trafic HTTP avec Azure CLI :

1. **Ouvrir votre terminal Azure CLI :**
   - Si vous n'avez pas encore Azure CLI installé, vous pouvez le télécharger et l'installer à partir de [ce lien](https://docs.microsoft.com/fr-fr/cli/azure/install-azure-cli).

2. **Se connecter à votre compte Azure :**
   ```bash
   az login
   ```

3. **Créer une nouvelle règle de sécurité pour autoriser le trafic HTTP :**
   Utilisez la commande suivante en remplaçant `<NomDuNSG>`, `<NomDuGroupeDeRessources>` et `<NomDeLaRègle>` par vos propres valeurs.

   ```bash
   az network nsg rule create \
     --resource-group <NomDuGroupeDeRessources> \
     --nsg-name <NomDuNSG> \
     --name <NomDeLaRègle> \
     --priority 310 \
     --direction Inbound \
     --access Allow \
     --protocol Tcp \
     --destination-port-ranges 80 \
     --source-port-ranges '*' \
     --source-address-prefixes '*' \
     --destination-address-prefixes '*'
   ```

### Exemple spécifique :

Si votre groupe de ressources s'appelle `myResourceGroup`, votre NSG s'appelle `HelloWorldVm-nsg`, et vous voulez créer une règle nommée `AllowHTTPInbound`, la commande serait :

```bash
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name HelloWorldVm-nsg \
  --name AllowHTTPInbound \
  --priority 310 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --destination-port-ranges 80 \
  --source-port-ranges '*' \
  --source-address-prefixes '*' \
  --destination-address-prefixes '*'
```

### Détails des Paramètres :

- `--resource-group`: Le nom de votre groupe de ressources Azure.
- `--nsg-name`: Le nom de votre groupe de sécurité réseau (NSG).
- `--name`: Le nom que vous souhaitez donner à la règle de sécurité.
- `--priority`: Le numéro de priorité pour la règle (plus le nombre est bas, plus la priorité est haute).
- `--direction`: La direction du trafic (`Inbound` pour le trafic entrant).
- `--access`: Le type d'accès (`Allow` pour autoriser le trafic).
- `--protocol`: Le protocole (`Tcp` dans ce cas, ou `Any` si vous voulez autoriser tous les protocoles).
- `--destination-port-ranges`: Les plages de ports de destination (ici, `80` pour HTTP).
- `--source-port-ranges`: Les plages de ports source (`*` pour tous les ports).
- `--source-address-prefixes`: Les préfixes d'adresses source (`*` pour toutes les adresses).
- `--destination-address-prefixes`: Les préfixes d'adresses de destination (`*` pour toutes les adresses).

En utilisant cette commande, vous allez créer une règle de sécurité dans votre NSG qui permet le trafic HTTP (port 80) à partir de n'importe quelle source.
