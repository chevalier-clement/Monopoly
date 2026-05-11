# Monopoly — Projet M1 Informatique
Projet réalisé par **Chevalier Clément** et **Frechin Zach** dans le cadre du Master 1 Informatique.

## Présentation

Implémentation en Java standard d'un jeu de Monopoly jouable à **2 joueurs** en mode console. L'objectif pédagogique est la mise en pratique des **patterns de programmation orientée objet**.

## Lancement

Compiler et exécuter la classe `Main`. Le menu principal propose :

- **Jouer** — lancer une partie à 2 joueurs
- **Options de démonstration** — scénarios préconfigurés pour illustrer les fonctionnalités du jeu (achat de propriétés, construction de maisons/hôtels, taxes, prison, etc.)

## Structure du projet

```
src/      Sources Java
lib/      Dépendances
bin/      Classes compilées (généré automatiquement)
```

## Patterns POO implémentés

### Héritage et classes abstraites

- **`Case`** — classe de base pour toutes les cases du plateau ; sous-classes concrètes : `Depart`, `Prison`, `Taxe`, `Fortune`, `Propriete`
- **`Propriete`** (abstraite) — spécialisée en `Rue`, `Gare` et `Service`, chacune avec sa propre logique de calcul de loyer

### Pattern Observateur (Observer)

- Interface **`Subject`** implémentée par `Propriete` : notifie les observateurs lors d'un changement d'état
- Interface **`Observer`** implémentée par `Quartier` : détecte quand un joueur possède toutes les rues d'un quartier pour débloquer la construction

### Pattern État (State)

Les rues (`Rue`) délèguent leur comportement à un objet **`EtatRue`** qui évolue au fil du jeu :

| État | Classe | Signification |
|------|--------|---------------|
| Libre | `EtatLibre` | Propriété disponible à l'achat |
| Achetée | `EtatAcheter` | Propriété possédée, pas encore constructible |
| Constructible | `EtatConstructible` | Tout le quartier est possédé, construction possible |
| Construite | `EtatConstruit` | Maisons ou hôtel présents |

### Pattern Méthode Template (Template Method)

La classe abstraite **`Menu`** définit la boucle d'interaction (`loop`) et délègue le traitement des choix à chaque sous-classe (`MainMenu`, `MenuTour`, `MenuAchat`, `MenuConstruction`, `MenuDemo`, etc.).

### Pattern Stratégie (Strategy)

Le calcul du loyer varie selon le type de propriété :

- `Rue` — loyer fixe, multiplié selon le nombre de maisons/hôtel
- `Gare` — loyer exponentiel selon le nombre de gares possédées
- `Service` — loyer proportionnel au résultat du lancer de dés

### Polymorphisme

Toutes les cases sont manipulées via leur type commun `Case` ; le comportement spécifique (atterrissage d'un joueur, calcul de loyer) est résolu dynamiquement par surcharge et redéfinition de méthodes.
