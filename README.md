# SNDS synthétique

Ce dépôt héberge une version synthétique du SNDS, publiées sous [licence ouverte] `etalab-2.0` (cf [texte](LICENCE.md)) .
 
Ces données sont générées avec la librairie [tsfaker](https://gitlab.com/healthdatahub/tsfaker), à partir du schéma du SNDS décrit dans [un dépôt GitLab dédié](https://gitlab.com/healthdatahub/schema-snds). 

Notes : 
- Ce schéma est également exposé dans un [dictionnaire interactif ](https://drees.shinyapps.io/dico-snds/), et via la section [Tables de la documentation collaborative](https://documentation-snds.health-data-hub.fr/tables/).
- Ce schéma est incomplet, ce qui se reflète dans les données synthétiques, en particulier par les colonnes qui sont des chaînes de caractère aléatoires lorsqu'elles ne sont pas associées à des nomenclatures.

## Organisation du dépôt

Le dossier `schemas` contient une version synthétiques des données du SNDS au format csv, ainsi que les schémas des tables au format table-schema (extension `.json). 

Le dossier `nomenclatures` est une copie du dossier correspondant dans le dépôt `schema-snds`.

## Génération automatique

Les fichiers de schéma bruts sont légèrement modifiés pour mettre toute l'information qu'ils contiennent au format [table-schema](https://gitlab.com/healthdatahub/schema-snds/blob/master/documentation/Table-Schema.md). 

En particulier, les nomenclatures indiquées pour chaque variables sont considérées comme des **clés étrangères** vers les tables de nomenclature, ce qui permet de sélectionner les valeurs dans ces tables.

La librairie python `tsfaker` (table schema faker) génére des données synthétiques à partir des fichiers de schéma et des tables de nomenclatures. 

À chaque modification du schéma sur la branche `master` du dépôt `schema-snds`, une nouvelle version des données est enregistrée sur ce dépôt. 

## Génération de nouvelles données

Vous pouvez télécharger ou cloner ce projet en local pour générer des données avec plus de lignes.

Il vous faudra installer la librairie `tsfaker` (commande `pip install tsfaker`).

Exécuter ensuite la commande suivante à la racine du projet. Cela écrasera les données synthétiques du projet par de nouvelles tables.
 
```bash
tsfaker schemas --resources nomenclatures --output schemas --nrows 10 --separator ',' --overwrite  --limit-fk 10
```

Paramètres à modifier : 
- `schemas` en indiquant des sous-dossiers pour génerer seulement une partie des tables ; 
- `nrows` pour générer un nombre différent de lignes partable ;
- `separator` pour changer le séparateur de champs ; 
- `output` pour écrire le résultat dans un autre dossier ;
- `limit-fk` pour choisir les valeurs des clés étrangères dans un plus grand nombre de lignes de la table référencée (supprimer l'option pour lire toute la table référencée) ;
