# AI Worker Standard 1.0

**Une norme ouverte de définition des AI Workers, AI Teams et AI Organizations.**

Par **Max Luo et Morgan Chen, cofondateurs de Teloa** · 18 octobre 2026

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

Un **AI Worker** est une entité de travail IA dont l’identité persiste entre les tâches, avec des responsabilités définies, un périmètre d’autorisation et des livrables vérifiables. Dans Teloa, le concept produit est celui d’**employé IA (AI employee)** ; AI Worker est le concept technique sous-jacent du standard. Grok Bot peut aussi être décrit ainsi, sous réserve des vérifications de correspondance ci-dessous.

## 1. Concepts essentiels

| Concept | Ce qu’il décrit | Son utilité |
| --- | --- | --- |
| AI Worker | Une entité identifiable avec des responsabilités, des contrats de travail et des contrôles des livrables | Le travail reste attribuable entre tâches, modèles et agents temporaires |
| AI Team | Un ensemble visant un résultat commun avec des rôles, des transmissions et une réception explicites | La collaboration repose sur un contrat de livraison vérifiable |
| AI Organization | Une composition de Workers et d’équipes avec responsabilité organisationnelle, limites de ressources et gestion des changements | Les équipes collaborent selon une gouvernance explicite |

Une identité persistante n’exige ni machine dédiée, ni processus permanent, ni mémoire à long terme. La responsabilité désigne ici l’attribution et la traçabilité du travail, pas une personnalité juridique. Une discussion de groupe ou une liste de membres ne constitue pas à elle seule un contrat d’équipe.

## 2. Périmètre et format

| Élément | Version 1.0 |
| --- | --- |
| Objets du noyau | `agent` et `organization` |
| Profil de travail obligatoire | `urn:teloa:profile:ai-work`, version `1.0` |
| Worker / Team / Organization | Worker : noyau + profil de travail ; Team : ajouter la collaboration ; Organization : ajouter la gouvernance |
| Échange | JSON ; rédaction facultative en YAML selon des règles de conversion restreintes |
| Vérifications du noyau | Huit règles communes R01–R08 ; six cas d’échange T01–T06 |
| Texte faisant autorité | Spécification du noyau et profil de travail en chinois simplifié ; traductions intégrales de lecture dans sept autres langues |

Le standard définit les identités, les révisions immuables, les références, les responsabilités, les interfaces de travail, la collaboration et la gouvernance. Les hôtes assurent l’exécution des modèles, la planification, les connexions et les autorisations. Importer une définition n’installe pas d’outils, ne connecte pas de comptes, ne lance pas de travail et n’active pas de routines. Un document `agent` limité aux six champs du noyau n’est pas une définition complète d’AI Worker.

## 3. Lire le standard

| Document | Contenu |
| --- | --- |
| [Guide avec schémas et texte consultable](docs/guide.fr.md) | Composition, architecture, déroulement de la livraison et exemple détaillé |
| [Spécification intégrale du noyau](spec/core.fr.md) | Champs, révisions fixes, références, règles communes et cas d’échange |
| [Profil intégral de travail et de collaboration](spec/work-profile.fr.md) | Contrats, ressources, transmissions, routines, liaisons d’instances et gouvernance |
| [Exemples](examples/README.md) | Worker complet en JSON/YAML, exemple de composition et entrées/sorties |
| [Correspondances avec les produits et spécifications](docs/mappings.md) | Teloa, Grok Bot, OASF et Oracle Agent Spec |
| [Conformité](CONFORMANCE.md) · [Gouvernance](GOVERNANCE.md) | Portée des preuves, contributions et maintenance |
| [Les huit langues](docs/languages.md) | Éditions intégrales de lecture et règles de traduction |

Chaque guide explique également les schémas dans le texte. Les traductions conservent les noms de champs, identifiants de règles et exemples JSON. Elles n’ont pas encore fait l’objet d’une révision complète par des spécialistes du domaine de langue maternelle. En cas de divergence, les deux documents normatifs chinois font autorité. Les documents communs de correspondance, d’exemples et de gouvernance sont en anglais et/ou chinois, comme indiqué dans l’index des langues.

## 4. Commencer par un Worker complet

1. Lire le [guide de composition et de livraison](docs/guide.fr.md).
2. Examiner [document-worker.json](examples/document-worker.json) ou son équivalent [YAML](examples/document-worker.yaml). Il reçoit `material` et renvoie `summary` et `openQuestions`.
3. Vérifier les responsabilités, schémas d’entrée/sortie, ressources nécessaires et contrat de réception ; comparer l’[entrée](examples/document-input.json) et la [sortie](examples/document-output.json) d’exemple.
4. Pour l’exécuter, lier la définition à une instance réelle, aux ressources prises en charge et à des autorisations effectives dans un hôte. Consigner séparément le résultat, sa réception et son adoption métier.

Les exemples illustrent les contrats ; ce ne sont ni un paquet de déploiement ni un validateur de conformité. L’exemple de composition contient des liaisons non résolues et n’est pas prêt pour une exécution réelle.

## 5. Noms de produits et spécifications externes

Le concept produit proposé aux utilisateurs de Teloa reste l’**employé IA**. La définition Worker décrit le rôle et le contrat de travail ; une instance Worker identifie un employé IA précis. Une équipe IA organise la collaboration et la livraison au moyen d’un contrat Team.

Le **Bot** de Grok peut correspondre à un AI Worker. Un **Team Bot** partagé entre personnes correspond d’abord à un Worker partagé, pas automatiquement à une équipe de plusieurs Workers. La conformité dépend des définitions et du comportement de l’hôte, pas du nom. Voir le [guide de correspondance](docs/mappings.md) pour les sources et vérifications.

OASF inspire les métadonnées de découverte et les taxonomies. Oracle Agent Spec inspire les références de composants, les interfaces et la composition. Cela n’établit pas une compatibilité complète de format ou d’exécution.

## 6. Statut, contributions et citation

Il s’agit d’une **spécification ouverte maintenue par Teloa**, pas d’une norme internationale adoptée ni d’un programme de certification. Aucun soutien de xAI, AGNTCY, Oracle ou d’un organisme de normalisation n’est revendiqué. L’interopérabilité indépendante exige des preuves reproductibles provenant d’implémentations indépendantes.

Soumettre les ambiguïtés concrètes, contre-exemples et rapports d’implémentation avec versions dans les [Issues](https://github.com/teloa-ai/ai-worker-standard/issues). Voir [GOVERNANCE.md](GOVERNANCE.md) pour les changements et [CONFORMANCE.md](CONFORMANCE.md) pour les déclarations.

Citation proposée : **Max Luo and Morgan Chen. AI Worker Standard 1.0. Teloa, 2026.** Citation lisible par machine : [CITATION.cff](CITATION.cff).

## 7. Licence

Les contenus originaux sont sous licence [Apache-2.0](LICENSE). Les noms de produits et les spécifications tierces conservent leurs droits respectifs. Aucun compte ni service Teloa n’est requis pour lire ou implémenter ce standard.
