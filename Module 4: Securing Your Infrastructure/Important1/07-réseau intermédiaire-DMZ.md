# Question 
Comment appelle-t-on le concept de mettre un réseau intermédiaire avant le réseau principal pour que ce réseau intermédiaire puisse être attaqué à la place du réseau principal ?

### Réponse Vulgarisée
Ce concept s'appelle une **DMZ (zone démilitarisée)**. 

### Explication Vulgarisée
Une DMZ est comme une zone tampon entre Internet (le monde extérieur) et le réseau interne de votre organisation (votre maison). Imaginez que vous avez des services que vous voulez rendre accessibles au public, comme un site web ou un serveur de messagerie. Vous ne voulez pas que les personnes de l'extérieur aient un accès direct à votre réseau interne, car cela pourrait être dangereux.

### Voici comment cela fonctionne :

1. **Réseau interne** : C'est votre réseau principal, où vous gardez toutes vos informations sensibles et importantes. C'est comme votre maison.

2. **Internet** : C'est le monde extérieur, où tout le monde peut essayer d'accéder à vos services publics.

3. **DMZ** : C'est une zone spéciale entre votre maison et le monde extérieur. C'est comme une véranda ou une avant-cour où vous pouvez recevoir des visiteurs sans les laisser entrer directement dans votre maison. 

### Pourquoi utiliser une DMZ ?

- **Isolation** : Les services publics (comme les sites web) sont placés dans la DMZ, ce qui empêche les attaquants d'accéder directement à votre réseau interne.
- **Sécurité** : Le trafic entre Internet et la DMZ est contrôlé par des pare-feu, et le trafic entre la DMZ et le réseau interne est également très limité.
- **Réduction des risques** : Si un attaquant parvient à pénétrer dans la DMZ, il ne pourra pas facilement accéder au réseau interne.

### Exemple de mise en place d'une DMZ

1. **Pare-feu extérieur** : Il filtre le trafic venant d'Internet vers la DMZ, ne permettant que les connexions nécessaires.
2. **Pare-feu intérieur** : Il contrôle le trafic entre la DMZ et le réseau interne, n'autorisant que les connexions spécifiques.

### Schéma Simplifié
```
Internet --- Pare-feu extérieur --- DMZ --- Pare-feu intérieur --- Réseau interne
              (site web)                      (base de données)
```

### Illustration avec iptables (sur Linux)

Pour mettre en place ces règles avec `iptables` (un outil de pare-feu pour Linux) :

1. **Pare-feu extérieur** :
```bash
# Autoriser l'accès au site web dans la DMZ
iptables -A INPUT -p tcp --dport 80 -d 192.168.1.10 -j ACCEPT
```

2. **Pare-feu intérieur** :
```bash
# Autoriser le site web de la DMZ à communiquer avec la base de données interne
iptables -A FORWARD -p tcp -s 192.168.1.10 --dport 3306 -d 192.168.2.10 -j ACCEPT
```

Ainsi, une DMZ permet de protéger votre réseau interne en ajoutant une couche de sécurité supplémentaire.
