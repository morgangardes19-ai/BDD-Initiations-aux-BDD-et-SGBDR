### ETAPE 1 : Obtenir l’utilisateur ayant le prénom “Muriel” et le mot de passe “test11”, sachant que l’encodage du mot de passe est effectué avec l’algorithme Sha1.
```sql
SELECT client.id, client.prenom, client.password
FROM client
WHERE prenom = 'Muriel' AND password = SHA1('test11');
```

### ETAPE 2 : Obtenir la liste de tous les produits qui sont présent sur plusieurs commandes.
```sql
SELECT commande_ligne.nom,
COUNT(*) AS nb_produits
FROM commande_ligne
GROUP BY commande_ligne.nom
HAVING nb_produits > 1
```


### ETAPE 3 : Obtenir la liste de tous les produits qui sont présent sur plusieurs commandes et y ajouter une colonne qui liste les identifiants des commandes associées.
```sql
SELECT commande_ligne.nom, GROUP_CONCAT(commande_ligne.commande_id) AS liste_identifiants,
COUNT(*) AS nb_produits
FROM commande_ligne
GROUP BY commande_ligne.nom
HAVING nb_produits > 1
```


### ETAPE 4 : Enregistrer le prix total à l’intérieur de chaque ligne des commandes, en fonction du prix unitaire et de la quantité.
```sql
UPDATE commande_ligne
SET commande_ligne.prix_total = commande_ligne.quantite * commande_ligne.prix_unitaire

"WHERE ====> Est-ce qu'il faut un WHERE ... = (*)? ==> on fait un GROUP BY obligatirement ?"
```


### ETAPE 5 : Obtenir le montant total pour chaque commande et y voir facilement la date associée à cette commande ainsi que le prénom et nom du client associé.
```sql
SELECT client.prenom, client.nom, commande.date_achat, commande_ligne.prix_total
FROM client
JOIN commande ON commande.id = client.id
JOIN commande_ligne ON commande_ligne.id = commande.id
```


### ETAPE 6 : (difficulté très haute) Enregistrer le montant total de chaque commande dans le champ intitulé “cache_prix_total”.
```sql
SELECT SUM(commande_ligne.prix_total) AS somme_prix_totaux
FROM commande_ligne
JOIN commande ON commande.id = commande_ligne.commande_id
GROUP BY commande_ligne.commande_id;
UPDATE commande
-------
FROM commande_ligne
INNER JOIN commande ON commande.id = commande_ligne.commande_id
-------
SET commande.cache_prix_total = somme_prix_totaux
"WHERE commande.cache_prix_total"

"WHERE commande_ligne.commande_id = * ===> remplacé par le GROUP BY ci-dessus ?"
```