# État de l'art

Ce document résume les trois références sur lesquelles s'appuie SynthUsers :
les agents génératifs (comment un LLM peut jouer un humain), les heuristiques
de Nielsen (comment classer un problème d'utilisabilité) et le questionnaire SUS
(comment mesurer la facilité d'usage perçue).

---

## 1. Generative Agents (Park et al., 2023)

**Référence :** J. S. Park, J. C. O'Brien, C. J. Cai, M. R. Morris, P. Liang,
M. S. Bernstein. *Generative Agents: Interactive Simulacra of Human Behavior.*
UIST 2023 (Stanford & Google). arXiv:2304.03442.

### L'idée

25 personnages vivent dans un petit village virtuel (« Smallville »). Chacun est
piloté par un LLM. Ils se réveillent, travaillent, discutent, et organisent même
ensemble une fête de la Saint-Valentin sans qu'on le leur demande.

### Les trois briques de l'architecture

```
        Perception (ce que l'agent voit)
                     │
                     ▼
┌─────────────────────────────────────────┐
│  FLUX DE MÉMOIRE (memory stream)        │
│  liste datée de toutes les observations │
└──────┬─────────────────────┬────────────┘
       │ récupération        │
       │ (récence +          ▼
       │  importance +   RÉFLEXION
       │  pertinence)    « qu'est-ce que j'en conclus ? »
       ▼                     │
   PLANIFICATION ◄───────────┘
   « que vais-je faire ? »
       │
       ▼
     Action
```

| Brique | Rôle | Exemple |
|---|---|---|
| Mémoire | Tout garder, puis retrouver les souvenirs utiles | « J'ai vu Maria au café hier » |
| Réflexion | Tirer des conclusions de haut niveau | « Klaus est passionné par sa recherche » |
| Planification | Découper la journée en actions | « 8h : petit-déjeuner, 9h : écrire » |

### Ce qu'on en retient pour SynthUsers

| Park et al. | SynthUsers |
|---|---|
| Fiche d'identité du personnage | **Persona** YAML (âge, aisance numérique, patience) |
| Observation de l'environnement | **Texte visible** de la page extrait par Playwright |
| Mémoire | Historique des pages et actions déjà faites |
| Réflexion | Commentaire : « qu'est-ce qui me gêne ? » |
| Action | Cliquer, remplir, revenir, abandonner |

### Limites signalées

Les agents sont parfois **trop polis et coopératifs**, ils peuvent **inventer**
des souvenirs, et leur crédibilité a été évaluée par des humains, pas comparée à
de vrais comportements. C'est exactement le point que SynthUsers veut mesurer :
un agent « trop coopératif » risque de réussir une tâche là où un vrai
utilisateur abandonnerait.

---

## 2. Les 10 heuristiques de Nielsen (1994)

**Référence :** J. Nielsen. *Enhancing the explanatory power of usability
heuristics.* CHI 1994 ; mises à jour sur nngroup.com.

Ce sont 10 règles générales pour évaluer une interface. L'agent analyste les
utilisera pour **classer** chaque problème signalé par les agents-personas.

| # | Heuristique | En une phrase | Exemple dans la version A |
|---|---|---|---|
| H1 | Visibilité de l'état du système | L'utilisateur sait où il en est | Pas de barre de progression |
| H2 | Correspondance avec le monde réel | Parler la langue de l'utilisateur | « Quotient familial » non expliqué |
| H3 | Contrôle et liberté | Pouvoir annuler, revenir | Pas de bouton « Retour » |
| H4 | Cohérence et standards | Mêmes mots, mêmes conventions | Deux libellés pour le même champ |
| H5 | Prévention des erreurs | Empêcher l'erreur avant qu'elle arrive | Aucun format indiqué pour le téléphone |
| H6 | Reconnaître plutôt que se souvenir | Ne pas obliger à mémoriser | Infos de l'accueil absentes du formulaire |
| H7 | Flexibilité et efficacité | Raccourcis pour les experts | — |
| H8 | Design esthétique et minimaliste | Pas d'information inutile | 15 champs sur une seule page |
| H9 | Aider à comprendre et corriger les erreurs | Message clair + solution | « Erreur de saisie » |
| H10 | Aide et documentation | Aide disponible si besoin | Pas de bulle d'aide |

### Échelle de gravité

Nielsen propose une note de 0 à 4. On la simplifie en trois niveaux :

| SynthUsers | Nielsen | Signification |
|---|---|---|
| **Bloquant** | 4 (catastrophe) | La tâche ne peut pas être terminée |
| **Gênant** | 2–3 | Ralentit ou frustre, mais la tâche aboutit |
| **Mineur** | 1 | Cosmétique |

### Pourquoi « 5 utilisateurs » ?

Nielsen & Landauer (1993) modélisent la découverte de problèmes ainsi :
`trouvés(n) = N × (1 − (1 − L)^n)`, avec `L ≈ 0,31` (part des problèmes trouvée
par un seul utilisateur).

| Utilisateurs n | 1 | 3 | 5 | 10 |
|---|---|---|---|---|
| Problèmes trouvés | 31 % | 67 % | **84 %** | 98 % |

Avec 5 personnes on trouve déjà la majorité des problèmes, **à condition que les
profils soient variés**. C'est là que les personas synthétiques peuvent aider :
couvrir des profils difficiles à recruter.

---

## 3. Le questionnaire SUS (Brooke, 1996)

**Référence :** J. Brooke. *SUS: A "quick and dirty" usability scale.*
In *Usability Evaluation in Industry*, 1996.

10 affirmations, chacune notée de 1 (pas du tout d'accord) à 5 (tout à fait
d'accord). Les questions alternent positif / négatif.

| # | Affirmation (traduction) | Sens |
|---|---|---|
| 1 | Je pense que j'aimerais utiliser ce système fréquemment | + |
| 2 | J'ai trouvé ce système inutilement complexe | − |
| 3 | J'ai trouvé ce système facile à utiliser | + |
| 4 | J'aurais besoin d'aide technique pour l'utiliser | − |
| 5 | Les fonctions du système sont bien intégrées | + |
| 6 | Il y a trop d'incohérences dans ce système | − |
| 7 | La plupart des gens apprendraient vite à l'utiliser | + |
| 8 | J'ai trouvé ce système très lourd à utiliser | − |
| 9 | Je me suis senti(e) en confiance en l'utilisant | + |
| 10 | J'ai dû apprendre beaucoup de choses avant de pouvoir l'utiliser | − |

### Calcul du score (0 à 100)

1. Questions impaires (positives) : `note − 1`
2. Questions paires (négatives) : `5 − note`
3. Additionner les 10 valeurs (0 à 40) et multiplier par **2,5**

**Exemple :** réponses `4, 2, 4, 1, 3, 2, 5, 1, 4, 2`

| Q | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| Note | 4 | 2 | 4 | 1 | 3 | 2 | 5 | 1 | 4 | 2 |
| Contribution | 3 | 3 | 3 | 4 | 2 | 3 | 4 | 4 | 3 | 3 |

Somme = 32 → SUS = 32 × 2,5 = **80**.

### Interprétation

- La moyenne observée sur de nombreuses études est d'environ **68**.
- Au-dessus de 68 : plutôt bon ; au-dessus de 80 : très bon ; sous 50 : problématique.
- Le SUS n'est **pas un pourcentage** : 68 signifie « dans la moyenne », pas « 68 % de satisfaction ».

### Usage dans SynthUsers

Chaque agent-persona remplit le SUS à la fin de sa tâche, en restant dans son
rôle. On compare ensuite les scores A vs B (moyenne + intervalle de confiance à
95 %) et les scores des agents vs ceux des vrais utilisateurs.

---

## 4. Ce que SynthUsers ajoute

| Question | Comment on y répond |
|---|---|
| Les agents trouvent-ils les vrais problèmes ? | **Rappel** sur la liste de référence de la version A |
| Inventent-ils des problèmes ? | **Précision** : problèmes réels / problèmes signalés |
| Préfèrent-ils la même version que les humains ? | Comparaison A/B agents vs 5–10 vrais utilisateurs |
| Où se trompent-ils ? | Analyse des écarts par persona et par heuristique |

**Hypothèse de départ :** les agents détecteront bien les problèmes « textuels »
(vocabulaire, messages d'erreur, H2 et H9), mais moins bien les problèmes
« perceptifs » (contraste, taille du texte, fatigue), car ils lisent le texte de
la page au lieu de la voir.
