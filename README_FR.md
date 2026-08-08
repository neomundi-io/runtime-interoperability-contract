🇬🇧 **English version:** [README.md](./README.md)

# Runtime Interoperability Contract

Sémantique minimale d’interopérabilité pour les signaux de mesure et de gouvernance runtime au sein d’infrastructures IA hétérogènes.

---

## Objectif

Ce dépôt définit un contrat minimal permettant l’échange de signaux runtime entre différentes couches indépendantes d’une infrastructure IA.

Il vise notamment l’interopérabilité entre :

* les couches de mesure et d’observabilité runtime,
* les systèmes de gouvernance et d’orchestration,
* les agents et architectures multi-agents,
* les systèmes d’audit et de forensic,
* les couches de qualification ou d’ancrage de preuve.

L’objectif **n’est pas** de définir un standard universel de gouvernance de l’IA.

L’objectif est d’établir un petit ensemble de sémantiques explicites, inspectables et lisibles par machine permettant à des systèmes indépendants d’échanger et d’interpréter des signaux runtime tout en conservant leur propre architecture, leur fonction et leur autorité.

---

## Principe fondamental

Une couche de mesure ne doit pas avoir besoin de contrôler l’infrastructure qu’elle observe.

Une couche de gouvernance ne doit pas avoir besoin d’implémenter elle-même le système de mesure qui produit ses signaux.

Une couche d’audit ou de preuve ne doit pas devenir l’autorité runtime.

Chaque couche peut rester indépendante tout en échangeant des signaux explicitement définis à travers un contrat de frontière commun.

Cela permet des architectures :

* composables,
* remplaçables,
* inspectables,
* extensibles,
* interopérables.

---

## Séparation des responsabilités

Le contrat repose sur une séparation stricte entre plusieurs fonctions.

### Mesure / OBS

Produit les mesures runtime et les signaux comportementaux.

Exemples :

* stabilité,
* `G`,
* `delta_G`,
* cohérence,
* variation comportementale,
* transitions de régime,
* anomalies runtime.

La mesure décrit un état ou une transition observée.

Elle ne détermine pas, à elle seule, ce qu’une application doit faire.

---

### Gouvernance / GOV

Consomme les signaux et applique des règles, politiques, seuils ou logiques d’orchestration propres à chaque application.

Les décisions possibles peuvent notamment être :

* `ALLOW`,
* `FLAG`,
* `REVIEW`,
* `BLOCK`,
* reroutage,
* fallback,
* escalade.

L’autorité de gouvernance reste extérieure à la couche de mesure.

---

### Systèmes runtime

Agents, orchestrateurs, applications, gateways, systèmes d’inférence ou autres infrastructures peuvent consommer les signaux selon leurs propres besoins.

Un même signal de mesure peut ainsi servir plusieurs usages indépendants.

---

### Couches de preuve / audit

Les systèmes de preuve peuvent conserver, qualifier, horodater, reconstruire ou ancrer des événements runtime.

L’observation et l’ancrage de preuve restent deux responsabilités distinctes.

---

## Principes d’interopérabilité

1. **L’autorité de mesure et l’autorité de gouvernance restent séparées.**

2. **L’observation runtime et l’ancrage de preuve restent séparés.**

3. **Chaque couche de mesure doit déclarer explicitement sa surface d’observation et ses limites.**

4. **Les signaux doivent rester interprétables à la fois par les machines et par les humains.**

5. **Les consommateurs restent responsables des décisions qu’ils dérivent des signaux de mesure.**

6. **Le contrat doit rester minimal avant de devenir complexe.**

7. **L’interopérabilité ne doit pas imposer une dépendance architecturale à un système particulier en amont ou en aval.**

---

## `measurement_surface`

Un signal runtime n’a de sens que si le système déclare également ce qu’il était réellement capable d’observer.

L’objet `measurement_surface` décrit cette frontière d’observation.

### Exemple minimal

```json
{
  "measurement_surface": {
    "visibility": "partial",
    "runtime_scope": "generation_stream",
    "observed_signals": [
      "G",
      "delta_G",
      "coherence"
    ],
    "limitations": [
      "no_access_to_model_weights",
      "no_internal_reasoning_visibility"
    ]
  }
}
```

`measurement_surface` ne prétend pas fournir une visibilité complète sur un modèle ou sur son raisonnement interne.

Son objectif est de déclarer explicitement la surface observable à partir de laquelle les mesures runtime sont produites.

La portée et les limites de chaque signal deviennent ainsi inspectables.

---

## Sémantique de frontière

Le contrat vise à décrire la frontière sémantique entre différentes couches indépendantes.

Une architecture simplifiée peut être représentée ainsi :

```text
SYSTÈME IA / AGENT / ORCHESTRATEUR
                │
                ▼
        EXÉCUTION RUNTIME
                │
                ▼
          MESURE / OBS
                │
         signaux runtime
                │
                ▼
   CONTRAT D’INTEROPÉRABILITÉ
        │         │         │
        ▼         ▼         ▼
      GOV       AUDIT    FORENSIC
        │
        ▼
   ACTION APPLICATIVE
```

La couche de mesure produit les signaux.

Le contrat d’interopérabilité définit comment ces signaux sont décrits et échangés.

Les autres infrastructures décident de la manière dont elles les utilisent.

---

## Pourquoi cette couche est importante

Une couche commune de mesure runtime peut servir plusieurs usages sans obliger ces usages à partager la même architecture.

Les mêmes signaux peuvent par exemple contribuer à :

* la surveillance runtime,
* la détection de dérive comportementale,
* la gouvernance IA,
* la supervision d’agents,
* l’évaluation de la préparation à la production,
* la reconstruction forensic,
* les processus d’audit,
* la surveillance post-déploiement,
* les systèmes d’optimisation.

Le contrat ne définit donc pas l’usage.

Il définit **la frontière à travers laquelle différents usages peuvent consommer un signal de mesure commun**.

---

## `/examples`

Ce répertoire pourra progressivement contenir des exemples minimaux de bout en bout tels que :

* `ALLOW`,
* `FLAG`,
* dérive comportementale,
* transitions de régime,
* dégradation de la visibilité de mesure,
* signaux non résolus.

Les exemples devront rester suffisamment simples pour pouvoir être inspectés et reproduits indépendamment.

---

## `/notes`

Les notes méthodologiques ouvertes pourront notamment porter sur :

* la continuité,
* `unresolved_signals`,
* la dégradation observationnelle,
* les perturbations runtime,
* les frontières de mesure,
* la compatibilité sémantique.

Ces notes restent exploratoires et ne doivent pas être automatiquement interprétées comme des éléments stabilisés du contrat.

---

## `/cross-layer`

Ce répertoire pourra documenter des relations expérimentales ou futures avec des concepts tels que :

* EVIDE,
* `visibility_surface`,
* `unresolved_signals`,
* les sémantiques de frontière,
* la qualification de preuve,
* la reconstruction forensic.

Ces articulations devront préserver la séparation entre mesure, gouvernance, exécution et preuve.

---

## Philosophie de conception

Ce dépôt évite volontairement de chercher à décrire une théorie ou une architecture complète de l’intelligence artificielle.

Son périmètre est plus précis :

> Définir le plus petit contrat sémantique utile permettant aux signaux de mesure runtime de circuler proprement entre des infrastructures IA indépendantes.

Le contrat doit rester :

* simple,
* lisible,
* inspectable,
* extensible,
* indépendant des implémentations,
* méthodologiquement explicite.

---

## Carte conceptuelle

```text
OBS
→ mesure

GOV
→ politique / orchestration / décision

Runtime Signals
→ télémétrie comportementale

Runtime Interoperability Contract
→ sémantique de frontière
```

Ensemble, ces éléments peuvent s’articuler avec des infrastructures hétérogènes telles que :

* agents,
* orchestrateurs,
* systèmes de gouvernance,
* systèmes d’audit,
* couches forensic,
* plateformes de monitoring,
* moteurs d’optimisation.

---

## Statut

Ce dépôt est expérimental et évolutif.

La priorité immédiate est de :

1. stabiliser les concepts minimaux,
2. maintenir des frontières propres entre les responsabilités,
3. éviter toute affirmation dépassant la surface réellement observable,
4. publier des exemples simples et interopérables,
5. laisser les intégrations réelles révéler les sémantiques effectivement nécessaires.

La complexité ne doit être introduite que lorsqu’un besoin d’implémentation réel la justifie.

---

## Direction à long terme

L’objectif n’est pas de construire un système fermé.

Il est de rendre la mesure runtime **observable, consommable et interopérable** entre des infrastructures IA hétérogènes.

Si ce contrat minimal se révèle utile dans plusieurs implémentations indépendantes, ses sémantiques pourront progressivement devenir plus stables, réutilisables et, à terme, potentiellement standardisables.
