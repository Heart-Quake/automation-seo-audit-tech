# Architecture, SEO Audit Automator

## Modules

```text
streamlit_app.py
  -> UI Streamlit, upload CSV, scope, affichage, export PPTX

audit_engine.py
  -> contrats colonnes, définitions de tickets, parsing CSV, analyse déterministe

automation_seo_theme.py
  -> design system Automation SEO, logo, build marker
```

## Flux d'analyse

```text
upload CSV
  -> load_screaming_frog_csv
  -> analyze_seo_data
  -> resolve_columns
  -> build_scope_masks
  -> evaluate_ticket pour chaque TicketDefinition
  -> IssueRecord[]
  -> Streamlit tables/cards/PPTX
```

## Modèle de règle

Chaque règle est définie par `TicketDefinition` :

- `id` ;
- `category` ;
- `name` ;
- `scope` ;
- `tier` ;
- `required_columns` ;
- textes de constat, recommandation, explication SEO.

La règle devient non évaluable si une colonne requise n'est pas résolue.

## Modèle de sortie

`IssueRecord` contient :

- statut ;
- nombre d'URLs touchées ;
- pourcentage ;
- priorité ;
- complexité ;
- catégorie ;
- scope ;
- colonnes requises ;
- exemples d'URLs ;
- textes de restitution.

## UI Streamlit

`streamlit_app.py` :

- configure la page ;
- charge le design system ;
- affiche le hero ;
- expose la sélection de périmètre ;
- charge le CSV ;
- affiche aperçu, métriques, tableau et cartes ;
- génère le PPTX.

## Export PPTX

`build_pptx` crée une présentation avec :

- slide titre ;
- une slide par issue détectée ;
- métadonnées catégorie/scope/tier ;
- constat ;
- recommandation ;
- exemples ;
- priorité visuelle.

## Design system

L'UI dépend de :

- `automation_seo_theme.apply_automation_seo_theme()` ;
- `logo-sidebar-cream.png` ;
- `.tool-hero` ;
- `data-app-build`.

## Points de vigilance

- Ne pas mélanger logique de règle et affichage Streamlit.
- Toute nouvelle règle doit déclarer explicitement ses colonnes requises.
- Toute règle dépendante d'un export enrichi doit accepter le statut `not_evaluable`.
- Les seuils SEO doivent rester déterministes et documentés.
- Ajouter des tests unitaires avant d'élargir fortement le catalogue de règles.
