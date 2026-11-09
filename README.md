# SynthUsers — tests d'utilisabilité par agents-personas LLM

> **Statut : en cours de développement** — Partie 1/7 : contexte et état de l'art.
> Le projet est publié étape par étape (voir la [feuille de route](#feuille-de-route)).

## Contexte

En design centré sur l'humain, on teste une interface avec de vrais utilisateurs avant de la lancer. Ces tests sont précieux, mais :

- ils sont **longs et coûteux** (recruter, observer, analyser) ;
- ils portent souvent sur **peu de personnes** (5 à 10), qui ne représentent pas tous les profils (seniors, personnes peu à l'aise avec le numérique, personnes en situation de handicap…) ;
- ils arrivent souvent **tard**, quand les changements coûtent cher.

Depuis 2023, des travaux montrent que les grands modèles de langage (LLM) peuvent imiter des comportements humains crédibles ([Park et al., 2023](docs/etat-de-l-art.md#1-generative-agents-park-et-al-2023)). En UX, une idée en découle : les **utilisateurs synthétiques**, des agents IA qui jouent le rôle d'utilisateurs pour un premier test rapide d'une interface.

## Question de recherche

> Des agents LLM dotés de personas peuvent-ils détecter les mêmes problèmes d'utilisabilité que de vrais utilisateurs ? Si oui, lesquels, et dans quelles limites ?

## Objectifs

1. Modéliser des utilisateurs sous forme de **personas** (profil, objectifs, contraintes).
2. Faire réaliser une **vraie tâche** sur une interface web par ces agents.
3. Collecter leurs **difficultés** : blocages, incompréhensions, réussite ou abandon.
4. **Comparer deux versions (A/B)** d'un même parcours.
5. **Valider** les résultats face à de vrais utilisateurs (précision et rappel).
6. Formuler des **recommandations** et documenter honnêtement les **limites**.

## Terrain de test

Une démarche en ligne fictive, **« Demande de bourse d'études »**, construite exprès en deux versions :

| | Version A (volontairement imparfaite) | Version B (améliorée) |
|---|---|---|
| Accueil | Bouton « Commencer » peu visible | Bouton clair « Demander une bourse » |
| Formulaire | Une seule longue page, 15 champs | 3 étapes courtes + barre de progression |
| Vocabulaire | Termes administratifs | Mots simples + bulles d'aide |
| Erreurs | « Erreur de saisie » | « Le numéro doit contenir 8 chiffres » |
| Accessibilité | Texte gris clair, petits caractères | Bon contraste, texte plus grand |
| Fin | Pas de confirmation claire | Récapitulatif + numéro de dossier |

**Tâche :** « Déposer une demande de bourse avec les informations de votre profil. »

Les problèmes placés dans la version A forment une **liste de référence** : on sait exactement ce que les agents devraient trouver.

## Architecture prévue

```
① Personas (YAML) ──► ② Navigateur (Playwright) ──► ③ Agent-persona (LLM + LangGraph)
                                    ▲                          │ action + commentaire
                                    └──────── boucle ──────────┘ (succès ou abandon)
                                                               ▼
                         ⑤ Rapport + tableau de bord ◄── ④ Agent analyste
                            (Streamlit)                    (heuristiques de Nielsen)
```

## Indicateurs

| Indicateur | Définition |
|---|---|
| Taux de réussite | % d'agents qui terminent la tâche |
| Nombre d'actions | Clics et étapes nécessaires |
| Points de blocage | Pages où les agents abandonnent ou reviennent en arrière |
| Problèmes détectés | Classés par gravité (bloquant / gênant / mineur) |
| Score SUS simulé | Questionnaire System Usability Scale rempli par chaque agent |

## Feuille de route

- [x] **Partie 1** — Contexte et état de l'art ([docs/etat-de-l-art.md](docs/etat-de-l-art.md))
- [ ] **Partie 2** — Application « Demande de bourse » (versions A et B) + liste des problèmes de référence
- [ ] **Partie 3** — Personas (YAML)
- [ ] **Partie 4** — Navigateur Playwright + agent-persona LangGraph
- [ ] **Partie 5** — Agent analyste + calcul des indicateurs
- [ ] **Partie 6** — Exécutions A/B, intervalles de confiance, validation avec de vrais utilisateurs
- [ ] **Partie 7** — Tableau de bord Streamlit + résultats et limites

## Technologies (prévues)

Python · LangGraph · Groq / Ollama · Playwright · YAML · Pandas, NumPy, SciPy · Streamlit

## ⚠️ Éthique

Les utilisateurs synthétiques servent à un **premier filtre rapide**. Ils **ne remplacent pas** les tests avec de vrais utilisateurs. Les personas de ce projet sont fictifs et ne visent pas à caricaturer un groupe de personnes.

## Auteure

**Nihed Missar** — élève ingénieure en Software & AI Engineering, ENICarthage · [GitHub](https://github.com/Nihed27) · [Portfolio](https://nihed27.github.io/Portfolio/)
