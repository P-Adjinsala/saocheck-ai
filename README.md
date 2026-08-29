# 🛡️ SaoCheck AI

**AI-powered fact-checking support tool for French-speaking Central Africa (Chad & Cameroon)**

Developed as part of the **Code for Africa (CfA) AI for Good Fellowship**, in partnership with **[WenakLabs](https://wenaklabs.org/)** (N'Djamena, Chad).

---

## 🇫🇷 Présentation du projet

SaoCheck AI est un outil d'aide à la vérification des faits (fact-checking) conçu pour le contexte francophone d'Afrique centrale. Le projet explore plusieurs approches de classification automatique de la véracité d'affirmations, avant de converger vers une architecture cible combinant **récupération documentaire (RAG)** et **génération augmentée par LLM**, avec une boucle de validation humaine assurée par les vérificateurs de WenakLabs.

### ⚠️ Note méthodologique sur les données

En l'absence, à ce stade, d'un corpus tchadien/camerounais annoté pour la vérification de faits, les expérimentations de ce dépôt utilisent le jeu de données public **[X-FACT](https://huggingface.co/datasets/utahnlp/x-fact)** (configuration française, 198 observations) comme **banc d'essai** pour valider le pipeline technique de bout en bout — chargement des données, tokenisation, fine-tuning, évaluation — et pour objectiver, avec des chiffres concrets, la contrainte principale du projet : le manque de données annotées localement.

**X-FACT n'est pas le corpus de production visé pour SaoCheck.** Les résultats présentés ici sont une validation de faisabilité technique, pas une mesure de performance en conditions réelles.

---

## 🧪 Approches explorées

| # | Modèle | Approche | Résultat clé | Statut |
|---|--------|----------|---------------|--------|
| 1 | CamemBERT fine-tuné — 5 classes | Classification fine (false / complicated / partly true / mostly true / true) | Effondrement sur la classe majoritaire (accuracy 36,7 % constante) | Pipeline validé — modèle non exploitable |
| 2 | CamemBERT fine-tuné — binaire | Classification simplifiée (faux / non-faux) | F1 = 0,744 à la meilleure époque (accuracy 80 %) | Résultat exploitable pour un MVP restreint |
| 3 | LLM + RAG (architecture cible) | Récupération documentaire + génération augmentée + validation humaine | Ne dépend pas d'un grand volume de données annotées | Direction recommandée pour la suite du projet |

Le détail complet (configuration, métriques par époque, diagnostics) est disponible dans les documents de présentation du dépôt.

---

## 📁 Structure du dépôt

```
saocheck-ai/
├── notebooks/
│   └── SaoCheck_AI_XFACT_Training.ipynb   # Pipeline complet : chargement X-FACT → EDA → fine-tuning CamemBERT (5 classes + binaire)
├── docs/
│   ├── SaoCheck_AI_Presentation_Modeles.docx      # Note de présentation (FR)
│   └── SaoCheck_AI_Presentation_Models_EN.docx    # Model presentation brief (EN)
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 🏗️ Architecture cible (LLM + RAG)

```
Affirmation à vérifier
        │
        ▼
┌───────────────────┐
│  Triage (CamemBERT)│   Filtre léger : factuel vérifiable vs opinion/hors-sujet
└─────────┬──────────┘
          ▼
┌───────────────────┐
│  Récupération (RAG)│   Base vectorielle : WenakLabs, AFP Factuel, presse régionale
└─────────┬──────────┘
          ▼
┌───────────────────┐
│  Génération (LLM)  │   Verdict + justification citée à partir des preuves récupérées
└─────────┬──────────┘
          ▼
┌───────────────────┐
│ Validation humaine  │   Vérificateur WenakLabs — corrections → futur corpus annoté
└────────────────────┘
```

---

## ⚙️ Installation

```bash
git clone https://github.com/<votre-compte>/saocheck-ai.git
cd saocheck-ai
pip install -r requirements.txt
```

Le notebook est conçu pour tourner sur Google Colab (GPU recommandé, ex. Tesla T4) ou tout environnement Jupyter avec accès GPU.

---

## 🔭 Prochaines étapes

- [ ] Constitution d'une base documentaire régionale (WenakLabs, AFP Factuel, presse tchadienne/camerounaise) pour le volet RAG
- [ ] Mise en place de la boucle de validation humaine pour générer un corpus annoté local
- [ ] Intégration du classifieur binaire comme filtre de triage en amont du pipeline RAG
- [ ] Tests d'architecture LLM + RAG sur affirmations réelles

---

## 🤝 Contexte du projet

Ce projet s'inscrit dans le sprint d'octobre 2026 de la **CfA AI for Good Fellowship**, en partenariat avec **WenakLabs** (N'Djamena, Tchad), organisation civique partenaire au Tchad.

**Auteur :** Pantouin Adjinsala — Health Data Analyst & MEL Specialist, Full Stack Developer, CfA AI for Good Fellow
📧 pantouinadjinsala@gmail.com · 💼 [LinkedIn](https://www.linkedin.com/in/pantouin-adjinsala-2b1429207) · 🌐 [Portfolio](https://pantouinadjinsala.com)

---

## 📄 Licence

À définir selon les modalités du programme CfA AI for Good Fellowship.
