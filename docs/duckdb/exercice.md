# Traitement de données géospatiales DuckDB

## La présentation

<iframe src="presentation.pdf" width="100%" height="600px"></iframe>

## Exercice 1: Introduction avec la CLI DuckDB

### Télécharger l'éxécutable DuckDB

Depuis le [site officiel](https://duckdb.org/install/?platform=windows&environment=cli)

![Téléchargement de l'éxécutable](./img/cli.png)

### Lancer DuckDB depuis un terminal

Il faut depuis un terminal se placer dans le dossier ou vous avez téléchargé l'éxécutable, par exemple:

```bash
cd C:\Users\florent\
```

Puis lancer l'éxécutable

```bash
duckdb.exe
```

On arrive alors dans la console DuckDB (équivalent psql) pn peux taper du SQL.

```sql
SELECT 1 as number, 'a' as letter ; 
```

!!! info
    Avant de faire Entrer il faut toujours terminer l'instruction SQL par un point virgule `;`

### Installer l'extension spatial

```sql
INSTALL spatial; 
```

Puis charger l'extension

```sql
LOAD spatial;
```

> La documentation de l'extension spatial se trouve sur cette [page](https://duckdb.org/docs/stable/core_extensions/spatial/overview), on y retrouve la liste des [fonctions](https://duckdb.org/docs/stable/core_extensions/spatial/functions) disponible

### Créer une base de données

!!! warning Nous sommes sur une base temporaire
    Actuellement nous sommes dans une base temporaire, tout ce que nous éxécutions ne sera pas sauvegarder. Il faut donc que crééons une base de données pour garder notre travail.

```sql
.open C:\Users\florent\chemin\vers\ma\base.duckdb
```

### Créer une table

Cette [page](https://duckdb.org/docs/stable/sql/statements/create_table) de la documentation montre des exemples.

On va créer une table nommé `ville` avec un champs, `id` sera nore clé primaire et qui s'auto incrémente, un champs `nom`de type `VARCHAR` et un champs géométrique nommé `geom`.

!!! warning Attention, les colonnes de géométries n'ont pas de type.
    Une des particularités des colonnes de géométrie sur DuckDB spatial est l'absence de définition du type de géométrie (contrairement à PostGIS par exemple). Une même colonne de géométrie contiendra aussi bien des points, des lignes, des polygones, etc. On peux dans une même table par exemple mélanger des points et des lignes. Mais que cela soit possible je ne vous le conseil pas.

```sql
CREATE SEQUENCE seq_ville START 1;
CREATE TABLE ville (
    id   INTEGER DEFAULT nextval('seq_ville') PRIMARY KEY,
    nom  VARCHAR,
    geom GEOMETRY
);
```

!!! info
    Pour rappel, une colonne de géométrie n'a pas de système de projection dans sa définition. Conséquences, il n'y a :
    - pas de définition de projection comme contrainte pour une colonne
    - pas d'attribution d'une projection lors d'un export

La commande `DESCRIBE` permet d'obtenir la description de la structure d'une table.

```sql
D describe ville ; 
┌─────────────┬─────────────┬─────────┬─────────┬─────────┬─────────┐
│ column_name │ column_type │  null   │   key   │ default │  extra  │
│   varchar   │   varchar   │ varchar │ varchar │ varchar │ varchar │
├─────────────┼─────────────┼─────────┼─────────┼─────────┼─────────┤
│ id          │ INTEGER     │ NO      │ PRI     │ NULL    │ NULL    │
│ nom         │ VARCHAR     │ YES     │ NULL    │ NULL    │ NULL    │
│ geom        │ GEOMETRY    │ YES     │ NULL    │ NULL    │ NULL    │
└─────────────┴─────────────┴─────────┴─────────┴─────────┴─────────┘
```

La commande `SHOW TABLES` permet de lister les tables.

```sql
D SHOW TABLES ; 
┌─────────┐
│  name   │
│ varchar │
├─────────┤
│ ville   │
└─────────┘
```

On va maintenant insérer nos des données dans notre table.

```sql
INSERT INTO ville (nom, geom) VALUES
    ('Rennes',    ST_GeomFromText('POINT(-1.6778 48.1173)')),
    ('Marseille', ST_GeomFromText('POINT(5.3698 43.2965)')),
    ('Laval',     ST_GeomFromText('POINT(-0.7667 48.0667)'));
```

Regardons le résultat.

```sql
D select * from ville ; 
┌───────┬───────────┬─────────────────────────┐
│  id   │    nom    │          geom           │
│ int32 │  varchar  │        geometry         │
├───────┼───────────┼─────────────────────────┤
│     1 │ Rennes    │ POINT (-1.6778 48.1173) │
│     2 │ Marseille │ POINT (5.3698 43.2965)  │
│     3 │ Laval     │ POINT (-0.7667 48.0667) │
└───────┴───────────┴─────────────────────────┘
```

### Importer un CSV et créer la géométrie

!!! info Lire un csv
    La fonction `read_csv_auto` nous permet de pouvoir importer un CSV sans avoir à créer la table au préalable. Cette fonction détecte automatiquement la structure du CSV.

```sql
CREATE TABLE airports AS FROM read_csv_auto('https://davidmegginson.github.io/ourairports-data/airports.csv', HEADER=True, DELIM=',') ;
```

Il faut maintenant ajouter une colonne de geometry à notre table.

```sql
ALTER TABLE airports ADD COLUMN the_geom GEOMETRY ;
```

Puis on mets à jour cette colonne en créant un point à partir des colonnes `longitude_deg` et `latitude_deg`

```sql
UPDATE airports SET the_geom = ST_POINT(longitude_deg, latitude_deg) ;
```

## Exercice 2: Récupérer des données et effectuer nos premiers traitements géographiques



### Lancer l'interface Web de DuckDB

!!! tip On va sortir du terminal
    Pour un peu plus de confort on va réaliser cet exercice dans l'interface web fourni avec l'éxécutable DuckDB, un peu l'équivalent du `PgAdmin`de DuckDB.

Il faut depuis un terminal se placer dans le dossier ou vous avez téléchargé l'éxécutable, par exemple:

```bash
cd C:\Users\florent\
```

Puis lancer l'éxécutable avec l'option `-ui`

```bash
duckdb.exe -ui
```

Une fênetre va s'ouvrir dans votre navigateur web à l'adresse `http://localhost:4213/`

![alt text](./img/ui.png)

## Exercice 3: Utiliser le plugin QGIS et visualiser nos données

