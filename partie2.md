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
```


### ETAPE 5 : Obtenir le montant total pour chaque commande et y voir facilement la date associée à cette commande ainsi que le prénom et nom du client associé.
```sql
SELECT client.prenom, client.nom, commande.date_achat, SUM(commande_ligne.prix_total) AS somme
FROM commande_ligne
LEFT JOIN commande ON commande.id = commande_ligne.commande_id
LEFT JOIN client ON client.id = commande.client_id
GROUP BY commande_ligne.commande_id;
```


### ETAPE 6 : (difficulté très haute) Enregistrer le montant total de chaque commande dans le champ intitulé “cache_prix_total”.
```sql
UPDATE commande
JOIN (SELECT commande_ligne.commande_id, SUM(commande_ligne.prix_total) AS prix_commande
     FROM commande_ligne
     GROUP BY commande_ligne.commande_id) T2
     ON commande.id = T2.commande_id
SET commande.cache_prix_total = T2.prix_commande
```


### ETAPE 7 : Obtenir le montant global de toutes les commandes, pour chaque mois.
```sql
SELECT SUM(commande_ligne.prix_total) AS somme_prix_mois, MONTH(commande.date_achat) AS date_mois
FROM commande_ligne
JOIN commande ON commande.id = commande_ligne.commande_id
GROUP BY MONTH(commande.date_achat)
```


### ETAPE 8 : Obtenir la liste des 10 clients qui ont effectué le plus grand montant de commandes, et obtenir ce montant total pour chaque client.
```sql
SELECT client.prenom, client.nom, SUM(commande_ligne.prix_total) AS montant_commande
FROM commande_ligne
JOIN commande ON commande.id = commande_ligne.commande_id
JOIN client ON client.id = commande.id
GROUP BY commande_ligne.prix_total
ORDER BY montant_commande DESC
LIMIT 10
```


### ETAPE 9 : Obtenir le montant total des commandes pour chaque date>.
```sql
SELECT SUM(commande_ligne.prix_total) AS somme_prix_date, commande.date_achat
FROM commande_ligne
JOIN commande ON commande.id = commande_ligne.commande_id
GROUP BY commande.date_achat
```


### ETAPE 10 : Ajouter une colonne intitulée “category” à la table contenant les commandes. Cette colonne contiendra une valeur numérique.
```sql
ALTER TABLE commande
ADD category int
```


### ETAPE 11 : Enregistrer la valeur de la catégorie, en suivant les règles suivantes :
* “1” pour les commandes de moins de 200€
* “2” pour les commandes entre 200€ et 500€
* “3” pour les commandes entre 500€ et 1.000€
* “4” pour les commandes supérieures à 1.000€
```sql
 INSERT INTO commande (category) VALUES (1, 2, 3, 4);
 SELECT SUM(commande_ligne.prix_total) AS somme_prix_date
 FROM commande_ligne
 JOIN commande ON commande.id = commande_ligne.commande_id
 GROUP BY commande_ligne.commande_id
 HAVING SUM(commande_ligne.prix_total) <200, SUM(commande_ligne.prix_total) >= 200 && <= 500,  SUM(commande_ligne.prix_total) >= 500 && <= 1000,  SUM(commande_ligne.prix_total) > 1000
```


### ETAPE 12 : Créer une table intitulée “commande_category” qui contiendra le descriptif de ces catégories.
```sql
ALTER TABLE commande_category
ADD descriptif VARCHAR (255)
```


### ETAPE 13 : Insérer les 4 descriptifs de chaque catégorie au sein de la table précédemment créée.
```sql
INSERT INTO table (colonne1, colonne2) VALUES (valeur1, valeur2);

INSERT INTO commande_category (Indice, fourchette_prix)
 VALUES (1, < 200 €)
 "N'affiche pas de prix dans fourchete_prix"
```


### ETAPE 14 : Supprimer toutes les commandes (et les lignes des commandes) inférieur au 1er février 2019. Cela doit être effectué en 2 requêtes maximum.
```sql
DELETE FROM commande
WHERE commande.date_achat < 2019-02-01
```