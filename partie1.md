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
 SELECT departement_nom, departement_code
 FROM departement
 JOIN villes_france_free ON villes_france_free.ville_commune
 
 
```