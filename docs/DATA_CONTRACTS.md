# Contrats de données, SEO Audit Automator

## Entrée principale

L'application attend un export CSV Screaming Frog, typiquement issu de l'onglet `Internal > HTML` ou d'un export enrichi avec des colonnes GSC / URL Inspection / performance.

Le fichier est fourni par upload Streamlit. Aucun fichier client ne doit être versionné.

## Lecture CSV

`load_screaming_frog_csv` gère :

- détection d'encodage ;
- détection de séparateur ;
- détection de ligne d'en-tête sur les premières lignes ;
- normalisation partielle des noms de colonnes français/anglais.

## Colonne obligatoire minimale

| Clé canonique | Alias acceptés | Rôle |
|---|---|---|
| `url` | `Adresse`, `Address`, `URL`, `Url` | URL auditée |

Sans URL, l'analyse s'arrête avec une erreur.

## Aliases principaux

Les colonnes sont résolues via `COLUMN_ALIASES` dans `audit_engine.py`.

| Clé canonique | Alias typiques |
|---|---|
| `status_code` | `Code HTTP`, `Status Code`, `Status` |
| `indexability` | `Indexabilité`, `Indexability`, `Indexable` |
| `indexability_status` | `Statut d'indexabilité`, `Indexability Status` |
| `title_1` | `Title 1`, `Title` |
| `title_1_len` | `Longueur du Title 1`, `Title 1 Length`, `Title Length` |
| `title_1_px` | `Largeur en pixels du Title 1`, `Title 1 Pixel Width` |
| `meta_desc_1` | `Meta Description 1`, `Meta Description` |
| `meta_desc_1_len` | `Longueur de la Meta Description 1`, `Meta Description 1 Length` |
| `h1_1` | `H1-1`, `H1 1`, `H1` |
| `h1_2` | `H1-2`, `H1 2` |
| `canonical_1` | `Canonical Link Element 1`, `Canonical Link Element`, `Canonical` |
| `meta_robots_1` | `Meta Robots 1`, `Meta Robots` |
| `inlinks` | `Liens entrants`, `Inlinks` |
| `response_time` | `Temps de réponse`, `Response Time` |

## Scopes disponibles

L'interface expose `SCOPE_LABELS` :

- toutes les URLs ;
- URLs HTML ;
- URLs HTML indexables.

Le scope sélectionné impacte les métriques de chaque scénario.

## Statuts de sortie

Chaque scénario produit un `IssueRecord` :

| Statut | Signification |
|---|---|
| `detected` | issue détectée sur au moins une URL du scope |
| `ok` | scénario évalué, aucun problème détecté |
| `not_evaluable` | colonnes requises absentes |

## Catégories de scénarios

Catégories couvertes par les définitions actuelles :

- HTTP ;
- indexabilité ;
- robots ;
- canonical ;
- title ;
- H1/H2 ;
- meta descriptions ;
- contenu ;
- maillage ;
- performance ;
- images ;
- hreflang ;
- GSC / URL Inspection ;
- mobile / rich results / Core Web Vitals.

## Sorties

L'interface expose :

- tableau des scénarios ;
- cartes détaillées ;
- exemples d'URLs ;
- export PPTX des issues détectées.

Le PPTX contient :

- site ;
- périmètre ;
- date de génération ;
- constats ;
- recommandations ;
- exemples ;
- priorité.

## Limites

- Les règles non évaluables ne doivent pas être interprétées comme des absences de problème.
- Les exports Screaming Frog personnalisés peuvent renommer des colonnes hors alias.
- Les colonnes enrichies GSC / URL Inspection ne sont présentes que si l'utilisateur les a ajoutées à l'export.
- Les seuils sont codés dans `audit_engine.py`.
