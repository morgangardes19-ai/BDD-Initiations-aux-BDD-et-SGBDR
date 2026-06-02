# Initiation

## 
* liste
<p><strong>ici</strong></p>

```sql
SELECT * FROM `livre` WHERE 1

```

<p><strong>Exemple du formateur</strong></p>  

```sql
SELECT departement.`departement_nom`, departement.departement_code, COUNT(villes_france_free.ville_slug) AS nombre_villes
FROM departement
JOIN villes_france_free ON departement.departement_code = villes_france_free.ville_departement
WHERE departement.departement_code = "01"
```

## Etape 1 : Liste décroissante avec une limite
```sql
 SELECT * 
 FROM villes_france_free 
 ORDER BY ville_population_2012 DESC
LIMIT 10
```

## Etape 2 : Liste croissante avec une limite
```sql
 SELECT * 
 FROM villes_france_free 
 ORDER BY ville_surface ASC
LIMIT 50
```

## Etape 3 : Liste à partir de 97.
```sql
 SELECT * 
 FROM departement 
WHERE departement_code LIKE '97%';
```

## Etape 4 : Associer 2 colonnes
```sql
 SELECT villes_france_free.ville_nom, departement.departement_nom
 FROM villes_france_free 
 JOIN departement ON departement.departement_code = villes_france_free.ville_departement
 ORDER BY ville_population_2012 DESC
LIMIT 10
```


## Etape 5 : à compléter ?
```sql
 SELECT departement.departement_nom, departement.departement_code,
 COUNT(villes_france_free.ville_commune) AS nombre_commune
 FROM departement
 JOIN villes_france_free ON villes_france_free.ville_departement = departement.departement_code
 GROUP BY departement.departement_nom
 ORDER BY nombre_commune DESC
```

## Etape 6 : 
```sql
 SELECT departement.departement_nom, departement.departement_code, 
SUM(villes_france_free.ville_surface) AS departement_superficie
 FROM departement
 JOIN villes_france_free ON villes_france_free.ville_departement = departement.departement_code
GROUP BY departement.departement_nom
ORDER BY departement_superficie DESC
LIMIT 10
```

## Etape 7 : 
```sql
 SELECT COUNT(villes_france_free.ville_nom) AS nb_ville_commencent_par_Saint
 FROM villes_france_free
 WHERE villes_france_free.ville_nom LIKE 'SAINT%';
```


## Etape 8 : 
```sql
 SELECT villes_france_free.ville_nom,
 COUNT(villes_france_free.ville_nom) AS nombre_ville_nom
 FROM villes_france_free
GROUP BY villes_france_free.ville_nom
ORDER BY nombre_ville_nom DESC
```


## Etape 9 : 
```sql
SELECT *
FROM villes_france_free
WHERE ville_surface > (SELECT AVG(ville_surface) FROM villes_france_free)
```

## Etape 10 : 
```sql
SELECT departement.departement_nom,
SUM(villes_france_free.ville_population_2012) AS population_2012
FROM departement
JOIN villes_france_free ON villes_france_free.ville_departement = departement.departement_code
GROUP BY departement.departement_nom
HAVING population_2012 > 2000000
```

## Etape 11 : 
```sql
UPDATE villes_france_free
SET villes_france_free.ville_nom = REPLACE(villes_france_free.ville_nom, 'SAINT-', 'SAINT ') 
WHERE villes_france_free.ville_nom LIKE 'SAINT-%';
```
