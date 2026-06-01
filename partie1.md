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

## Etape 1 :
```sql
 SELECT * 
 FROM villes_france_free 
 ORDER BY ville_population_2012 DESC
LIMIT 10
```

## Etape 2 :
```sql
 SELECT * 
 FROM villes_france_free 
 ORDER BY ville_surface ASC
LIMIT 50
```