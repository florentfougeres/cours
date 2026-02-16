# Traitement de données géospatiales avec Parquet et DuckDB

## Exercice 1: Utilisation de la CLI DuckDB

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

> Avant de faire Entrer il faut toujours terminer l'instruction SQL par un point virgule `;`

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

!!! warning
    Actuellement nous sommes dans une base temporaire, tout ce que nous éxécutions ne sera pas sauvegarder. Il faut donc que crééons une base de données pour garder notre travail.

```sql
.open C:\Users\florent\chemin\vers\ma\base.duckdb
```

### Créer une table

Cette [page](https://duckdb.org/docs/stable/sql/statements/create_table) de la documentation montre des exemples.

