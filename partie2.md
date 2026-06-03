### ETAPE 1 : Obtenir l’utilisateur ayant le prénom “Muriel” et le mot de passe “test11”, sachant que l’encodage du mot de passe est effectué avec l’algorithme Sha1.
```sql
SELECT client.id, client.prenom, client.password
FROM client
WHERE prenom = 'Muriel' AND password = SHA1('test11');
```

### ETAPE 2 : Obtenir la liste de tous les produits qui sont présent sur plusieurs commandes.
```sql
SELECT commande_ligne.nom,
COUNT(commande_ligne.quantite) AS nb_produits
FROM commande_ligne
JOIN commande ON commande.id = commande_ligne.id
GROUP BY commande_ligne.nom
HAVING nb_produits > 1
```


### ETAPE 3 : Obtenir la liste de tous les produits qui sont présent sur plusieurs commandes et y ajouter une colonne qui liste les identifiants des commandes associées.
```sql

```