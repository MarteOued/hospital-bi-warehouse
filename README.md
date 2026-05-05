<h1 align="center">🏥 Hospital BI Warehouse</h1>

<p align="center">
  <i>Système décisionnel complet pour l'analyse des consultations médicales —
  ETL Python + modèle en étoile + dashboards Tableau.</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Pandas-2.0+-150458?style=flat&logo=pandas&logoColor=white" />
  <img src="https://img.shields.io/badge/Tableau-Desktop-E97627?style=flat&logo=tableau&logoColor=white" />
  <img src="https://img.shields.io/badge/Schema-Star%20Model-success?style=flat" />
  <img src="https://img.shields.io/badge/OLAP-3%20Dashboards-blueviolet?style=flat" />
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=flat" />
</p>

<p align="center">
  <a href="#-aperçu">🎯 Aperçu</a> ·
  <a href="#-modèle-en-étoile">⭐ Modèle</a> ·
  <a href="#-dashboards">📊 Dashboards</a> ·
  <a href="#-quick-start">🚀 Quick Start</a> ·
  <a href="https://portfoliomarte.vercel.app">🌐 Portfolio</a>
</p>

---

## 🎯 Aperçu

Ce projet conçoit un **entrepôt de données décisionnel** à partir de données hospitalières
relatives aux consultations médicales. Il couvre **toute la chaîne BI** :
nettoyage des données brutes, modélisation en étoile, enrichissement par hiérarchies et KPI,
implémentation dans Tableau, et création de dashboards stratégiques.

> **Logique BI suivie** : *Données brutes → Prétraitement → Modèle → Analyse → Décision*

**Objectifs métier :**
- 📊 Suivre l'**activité médicale** (volumes de consultations, durées, coûts)
- 🩺 Identifier les **pathologies les plus fréquentes** par spécialité
- 💰 Optimiser l'**allocation des ressources** (services, médecins, créneaux)
- 📈 Détecter les **tendances temporelles** (saisonnalité, pics d'activité)

## ✨ Fonctionnalités

- 🧹 **Pipeline ETL Python** complet (Pandas, NumPy) — 84 cellules documentées
- ⭐ **Modèle en étoile** : 1 table de faits + 6 dimensions
- 🆕 **Dimensions créées** : `DIM_DIAGNOSTIC` et `DIM_TRAITEMENT` extraites par normalisation
- 📐 **Enrichissement** : tranches d'âge patients, catégories d'ancienneté médecins, hiérarchies temporelles (Année → Trimestre → Mois)
- ✅ **Vérifications qualité** : intégrité référentielle, doublons, valeurs aberrantes, formats
- 📊 **3 dashboards Tableau** : vue globale, analyse médicale, analyse financière
- 📈 **KPI métier** : coût total, durée moyenne, top services, classement médecins
- 🎓 **Justifications stratégiques** pour chaque enrichissement et chaque KPI

## ⭐ Modèle en étoile

```
                     ┌─────────────────┐
                     │  DIM_PATIENTS   │
                     │ ─────────────── │
                     │ ID_Patient      │
                     │ Date_Naissance  │
                     │ Age             │
                     │ Tranche_Age     │
                     │ Sexe, Ville     │
                     └────────┬────────┘
                              │
   ┌──────────────────┐       │      ┌──────────────────┐
   │ DIM_DIAGNOSTIC   │       │      │ DIM_DOCTEURS     │
   │ ──────────────── │       │      │ ──────────────── │
   │ ID_Diagnostic    │       │      │ ID_Docteur       │
   │ Diagnostic       │       │      │ Spécialité       │
   └────────┬─────────┘       │      │ Ancienneté       │
            │                 │      │ Catégorie_Anc.   │
            │                 │      └────────┬─────────┘
            │                 │               │
            │  ┌──────────────▼───────────────┘
            └──┤   FACT_CONSULTATIONS       │
               │ ──────────────────────────  │
               │ ID_Consultation             │
               │ ID_Patient                  │
               │ ID_Docteur                  │
               │ ID_Service                  │
               │ ID_Temps                    │
               │ ID_Diagnostic               │
               │ ID_Traitement               │
               │ Coût (€)                    │
               │ Durée (min)                 │
               └──┬──────────────┬───────────┘
                  │              │
   ┌──────────────▼─┐    ┌───────▼──────────┐    ┌─────────────────┐
   │ DIM_SERVICES   │    │    DIM_TEMPS     │    │ DIM_TRAITEMENT  │
   │ ────────────── │    │ ──────────────── │    │ ─────────────── │
   │ ID_Service     │    │ ID_Temps         │    │ ID_Traitement   │
   │ Nom_Service    │    │ Date             │    │ Traitement      │
   │ Type_Service   │    │ Année, Trimestre │    └─────────────────┘
   └────────────────┘    │ Mois, Semestre   │
                         │ Saison, Type_Jour│
                         └──────────────────┘
```

**Granularité** : 1 ligne de faits = 1 consultation médicale.

## 📊 Dashboards Tableau

> 📁 Workbook complet disponible dans `tableau/hospital_dashboards.twb`
> 🔗 *Version interactive disponible bientôt sur [Tableau Public](https://public.tableau.com/profile/martine.ouedraogo)*

### Dashboard 1 — Vue globale

| Indicateur | Détail |
|------------|--------|
| 🔢 Nombre total de consultations | KPI principal |
| 💰 Coût total | Vue financière |
| 📅 Évolution temporelle | Courbe mensuelle / annuelle |
| 🏥 Top services | Classement |

### Dashboard 2 — Analyse médicale

- Distribution **par spécialité** (Cardiologie, Pédiatrie, etc.)
- Distribution **par service**
- Distribution **par diagnostic** (Top pathologies)
- Cross-analyse **âge × diagnostic**

### Dashboard 3 — Analyse financière

- **Coût par mois** (saisonnalité)
- **Comparaison services** (coût moyen, volumes)
- **Indicateurs de performance** par médecin / service
- **Coût moyen par tranche d'âge** patient

## 🛠️ Stack technique

| Layer | Outils |
|-------|--------|
| **ETL / Data Engineering** | Python 3.10+ · Pandas · NumPy |
| **Storage** | CSV (entrée et sortie ETL) |
| **Modélisation** | Schéma en étoile (Fact + 6 dimensions) |
| **BI / Analytics** | Tableau Desktop |
| **Notebook** | Jupyter / Google Colab |

## 🚀 Quick Start

### 1. Cloner le repo

```bash
git clone https://github.com/MarteOued/hospital-bi-warehouse.git
cd hospital-bi-warehouse
```

### 2. Installer Python

```bash
pip install -r requirements.txt
```

### 3. Lancer le pipeline ETL

Ouvre le notebook dans Jupyter ou Google Colab :

```bash
jupyter notebook notebooks/01_ETL_data_warehouse.ipynb
```

Ce notebook :
1. Charge les 5 CSV bruts (`consultations`, `doctors`, `patients`, `services`, `time`)
2. Effectue l'analyse exploratoire et les vérifications qualité
3. Crée les dimensions enrichies (`DIM_DIAGNOSTIC`, `DIM_TRAITEMENT`, tranches d'âge, catégories d'ancienneté)
4. Exporte les 7 tables propres dans `data/processed/`

### 4. Ouvrir les dashboards Tableau

```
tableau/hospital_dashboards.twb
```

Dans Tableau Desktop : `Fichier → Ouvrir → hospital_dashboards.twb`

## 📂 Structure du projet

```
hospital-bi-warehouse/
├── notebooks/
│   └── 01_ETL_data_warehouse.ipynb     # Pipeline complet, 84 cellules
├── tableau/
│   └── hospital_dashboards.twb         # 3 dashboards Tableau
├── data/
│   ├── raw/                            # CSV bruts (à ajouter localement)
│   └── processed/                      # CSV nettoyés générés
├── screenshots/                        # Captures des dashboards
├── docs/
│   └── project_specification.pdf       # Cahier des charges
├── requirements.txt
├── .gitignore
└── README.md
```

## 🧠 Étapes méthodologiques

### Étape 1 — Pré-traitement (Python)

- Vérification des **doublons** et **nullités** sur chaque table
- Validation de l'**intégrité référentielle** (les ID dans `Consultations` existent dans les dimensions)
- Contrôle des **valeurs aberrantes** (Coût négatif, ancienneté > 60 ans, années 2000-2050)
- **Correction des formats** (dates, numériques, texte)

### Étape 2 — Modélisation initiale

- **Table de faits** : `Consultations`
- **Dimensions** : `Patients`, `Docteurs`, `Services`, `Temps`
- **Granularité** : 1 ligne = 1 consultation

### Étape 3 — Enrichissement (le cœur du projet)

#### A. Hiérarchies (Tableau)
- `Année > Trimestre > Mois`
- `Type_Service > Nom_Service`
- `Tranches d'âge` patients

#### B. KPI calculés (Tableau)
- Coût moyen par patient / par spécialité / par tranche d'âge
- Durée moyenne par service
- Nombre de consultations par spécialité
- Top 5 services les plus sollicités

#### C. Dimensions ajoutées (Python)
- **`DIM_DIAGNOSTIC`** — extraction des diagnostics uniques en table séparée
- **`DIM_TRAITEMENT`** — extraction des traitements uniques en table séparée

> Justification métier : "L'ajout d'une dimension Diagnostic permet d'identifier les pathologies
> les plus fréquentes et d'optimiser l'allocation des ressources médicales."

### Étape 4 — Implémentation Tableau

- Import des 7 tables propres
- Création des **relations** entre faits et dimensions
- Vérification des **agrégations** et **jointures**
- Création des **hiérarchies** Tableau

### Étape 5 — KPI & Analyses OLAP

**Indicateurs minimum** : coût total, nombre de consultations, durée moyenne.

**Indicateurs avancés** :
- Coût moyen par spécialité
- Évolution mensuelle / annuelle
- Analyse par tranche d'âge
- Classement des services et médecins

### Étape 6 — Dashboards & présentation

3 dashboards orientés décision :
1. **Vue globale** — exécutif
2. **Analyse médicale** — direction médicale
3. **Analyse financière** — direction administrative

## 👥 Auteurs

Projet réalisé en équipe dans le cadre du module **Systèmes d'Information Décisionnels**
du **Master 1 Informatique** — **Université Lumière Lyon 2** (2026).

- **Martine Ouedraogo** — ETL Python, modélisation, dashboards Tableau
  [LinkedIn](https://www.linkedin.com/in/marte-oued) · [Portfolio](https://portfoliomarte.vercel.app) · [GitHub](https://github.com/MarteOued)

## 📜 Licence

Projet académique — Université Lumière Lyon 2.
Données utilisées dans un cadre pédagogique.
