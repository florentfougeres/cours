# Mes cours

## Mkdocs

La documentation est générée avec MkDocs. Vous pouvez consulter la documentation officielle ici : [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

Pour visualiser la documentation localement :

```bash
python3 -m venv && . .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Ouvrez ensuite http://127.0.0.1:8000/ dans votre navigateur pour voir le site.

Pour générer la version statique :

```sh
mkdocs build
```

## Présentation en PDF

Les présentations sont en markdown dans le support et sont tranformé en PDF en utilisant marp pour publication dans le MkDocs. Cette opération est faites dans le CI.

Voici la commande utilisé:

```sh
marp docs/duckdb/presentation.md --pdf -o docs/duckdb/presentation.pdf
```

## Licence

Ce cours est mis à disposition sous licence **Creative Commons Attribution-NonCommercial (CC BY-NC 4.0)**.  

Cela signifie que vous pouvez :  
- **Partager** — copier et redistribuer le matériel sous n’importe quel format ou support  
- **Adapter** — remixer, transformer et construire à partir du matériel  

Sous les conditions suivantes :  
- **Attribution** — vous devez créditer l’auteur, fournir un lien vers la licence, et indiquer si des modifications ont été effectuées.  
- **NonCommercial** — vous n’êtes pas autorisé à utiliser ce matériel à des fins commerciales.  

Pour plus de détails, consultez la licence officielle : [https://creativecommons.org/licenses/by-nc/4.0/](https://creativecommons.org/licenses/by-nc/4.0/)