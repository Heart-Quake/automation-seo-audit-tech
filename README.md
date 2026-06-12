# SEO Audit Automator

Application Streamlit publique pour analyser des exports Screaming Frog et produire un audit technique SEO déterministe. L'outil transforme un CSV de crawl en liste de scénarios priorisés, avec constats, recommandations, scope, complexité et export PPTX.

## Source live

| Élément | Valeur |
|---|---|
| Live URL | https://automation-seo-audit-tech.streamlit.app/ |
| Repository principal | https://github.com/Heart-Quake/automation-seo-audit-tech |
| Miroir | https://github.com/YN-CodingClub/automation-seo-audit-tech |
| Branche live | `main` |
| Entrypoint Streamlit | `streamlit_app.py` |
| Commande locale | `streamlit run streamlit_app.py` |
| Compilation | `python3 -m py_compile streamlit_app.py audit_engine.py automation_seo_theme.py` |
| Tests | Aucun test automatisé à date |
| Build marker live vérifié | `automation-seo-audit-tech:0388781` |
| Secrets | Aucun secret requis |

## Rôle produit

L'outil sert à accélérer un pré-audit technique depuis Screaming Frog. Il ne crawl pas lui-même : il analyse uniquement le CSV fourni par l'utilisateur.

Il produit :

- une vue d'ensemble des scénarios détectés, OK ou non évaluables ;
- un tableau détaillé des issues ;
- des cartes d'audit avec constat, recommandation, explication SEO et exemples d'URLs ;
- un export PPTX pour transformer les issues détectées en support de restitution.

Hors périmètre :

- crawl live ;
- connexion GSC ou URL Inspection API ;
- correction automatique des URLs ;
- stockage d'exports client.

## Quickstart

```bash
cd /Users/vincentflaceliere/Github/automation-seo-audit-tech
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
streamlit run streamlit_app.py
```

Vérification minimale :

```bash
python3 -m py_compile streamlit_app.py audit_engine.py automation_seo_theme.py
```

## Documentation

- [Contrats de données](docs/DATA_CONTRACTS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Runbook Streamlit](docs/RUNBOOK.md)

## Flux fonctionnel

1. L'utilisateur dépose un export CSV Screaming Frog.
2. `load_screaming_frog_csv` détecte l'encodage, le séparateur et la ligne d'en-tête.
3. `analyze_seo_data` résout les colonnes disponibles et construit les scopes.
4. Chaque `TicketDefinition` est évalué selon ses colonnes requises.
5. Les résultats sont classés en `detected`, `ok` ou `not_evaluable`.
6. L'interface affiche métriques, tableaux, cartes et export PPTX.

## Design system

L'app doit rester alignée avec le design Automation SEO :

- `automation_seo_theme.py` chargé après `st.set_page_config`.
- `logo-sidebar-cream.png` présent à la racine.
- hero `.tool-hero` dans l'entrypoint.
- marqueur caché `data-app-build`.
- patterns interdits : `#2BAF9C`, `DR SEO`, `Dr. SEO`, `base = "light"`.

## Ajouter ou modifier une règle d'audit

1. Ajouter ou modifier un `TicketDefinition` dans `audit_engine.py`.
2. Déclarer les `required_columns` avec les clés canoniques de `COLUMN_ALIASES`.
3. Ajouter la logique d'évaluation dans `evaluate_ticket` si la règle n'est pas couverte par les helpers existants.
4. Vérifier que le scénario passe en `not_evaluable` si les colonnes manquent.
5. Lancer la compilation.
6. Ajouter un test unitaire dans un futur batch, car le repo n'a pas encore de suite de tests.

## Dette documentaire et tests

À créer dans un prochain lot :

- fixtures Screaming Frog anonymisées ;
- tests unitaires sur `audit_engine.py` ;
- documentation exhaustive `docs/AUDIT_RULES.md` listant chaque scénario.
