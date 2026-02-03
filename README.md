# Dash-Python Books — Code Listings

## English

This repository contains the **canonical code listings** for a series of
professional technical books about **Dash and Python**.

### Scope

- This repository contains **only code listings** appearing in the books.
- No tutorials, no additional explanations, no extra examples.
- The repository is **not** a Python package and is **not** meant to be installed.

### Repository model

- One directory per book.
- Each book directory contains **two Markdown files**:
  - `code.fr.md` (French)
  - `code.en-us.md` (American English)
- Both files contain the **exact same Python code listings**.
  Only the surrounding explanatory text differs.

The Markdown files are the **canonical source of truth** for all code examples.

### Code guarantees

- All Python code blocks are complete and self-contained.
- Code is presented exactly as it appears in the books.
- No refactoring or improvements outside published editions.

### Versioning

- Git tags correspond to published book editions.
- Tagged versions are immutable.
- The `main` branch may evolve between editions.

Example tags:
- `v1.0-book01`
- `v1.1-book01-errata`
- `v2.0-book02-release`

Each book explicitly references the corresponding tag.

### Issues policy

GitHub Issues are used **only** to report:
- errors in code listings,
- inconsistencies between the book and this repository.

Support requests, questions, or discussions are out of scope.

### License

The code listings are released under the **MIT License**.  
The text of the books is **not** included in this repository.

## Editions

Each published book edition corresponds to a specific state of this repository,
identified by a Git tag.

Readers should refer to the tag matching their book edition to access the exact
code listings used in that edition.

---

## Français

Ce dépôt contient les **listings de code canoniques** accompagnant une série
d’ouvrages techniques professionnels consacrés à **Dash et Python**.

### Périmètre

- Le dépôt contient **uniquement les listings de code** présents dans les livres.
- Aucun tutoriel, aucune explication supplémentaire, aucun exemple annexe.
- Le dépôt **n’est pas** un package Python et **n’a pas vocation** à être installé.

### Modèle du dépôt

- Un dossier par livre.
- Chaque dossier de livre contient **deux fichiers Markdown** :
  - `code.fr.md` (français)
  - `code.en-us.md` (anglais américain)
- Les deux fichiers contiennent **strictement le même code Python**.
  Seuls les textes explicatifs diffèrent.

Les fichiers Markdown constituent la **référence canonique** des exemples de code.

### Garanties sur le code

- Chaque bloc de code Python est complet et autonome.
- Le code est reproduit exactement tel qu’il apparaît dans les livres.
- Aucun refactoring ni amélioration hors des éditions publiées.

### Versionnement

- Les tags Git correspondent aux éditions publiées des livres.
- Les versions taguées sont immuables.
- La branche `main` peut évoluer entre deux éditions.

Exemples de tags :
- `v1.0-book01`
- `v1.1-book01-errata`
- `v2.0-book02-release`

Chaque livre indique explicitement le tag associé.

### Politique des Issues

Les Issues GitHub servent **exclusivement** à signaler :
- des erreurs dans les listings de code ;
- des incohérences entre le livre et le dépôt.

Les demandes de support, questions ou discussions ne sont pas prises en charge.

### Licence

Les exemples de code sont publiés sous licence **MIT**.  
Le texte des livres n’est **pas** inclus dans ce dépôt.

## Éditions

Chaque édition publiée correspond à un état spécifique de ce référentiel,
identifié par une balise Git.

Les lecteurs doivent se référer à la balise correspondant à leur édition pour accéder aux listes de code exactes
utilisées dans cette édition.
