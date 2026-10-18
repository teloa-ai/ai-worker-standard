# Profil de travail et de collaboration des AI Workers

[Languages / 语言](../docs/languages.md)

**Max Luo et Morgan Chen, cofondateurs de Teloa**

Date : 2026-10-18. Version : `1.0`. Annexe normative de la [Norme ouverte de définition des AI Workers, AI Teams et AI Organizations 1.0](core.fr.md). Elle définit la sémantique du travail et les exigences d’adaptation, sans définir un nouveau moteur d’exécution.

Cette traduction suit le [texte de référence en chinois simplifié](work-profile.zh-CN.md). Elle n’a pas encore fait l’objet d’une révision complète par des spécialistes du secteur de langue maternelle française. Veuillez signaler toute divergence avec le texte de référence. Les valeurs des exemples JSON et YAML sont conservées à l’identique pour permettre la comparaison entre langues.

## 1. Relier Worker, Team et Organization par un contrat commun

Un employé propose des points d’entrée de travail ; une équipe propose ses propres points d’entrée et explique la répartition entre membres ; une organisation poursuit la composition d’équipes. Tous trois précisent entrées, sorties, dépendances, responsabilités et réception. Une organisation n’expose pas automatiquement tous les travaux de ses membres, et y entrer ne partage pas automatiquement la mémoire personnelle.

Le profil de travail apparaît comme une extension obligatoire : `id = urn:teloa:profile:ai-work`, `version = "1.0"`, `required = true`. C’est l’identifiant fixe de cette spécification, pas un mécanisme d’authentification ni un accès à un service en ligne. Tout hôte peut l’implémenter sans compte Teloa. Il est interdit de le rendre optional pour contourner une sémantique essentielle. Un fichier ne contenant que les champs du noyau agent peut uniquement déclarer la prise en charge de l’échange du noyau, pas la conformité d’une définition complète d’AI Worker.

Les règles du noyau sur les chaînes non vides, LocalId, références fixes, import sans effets de bord et extensions inconnues restent applicables. Les structures suivantes n’acceptent que les champs indiqués, tous obligatoires sauf mention facultative. Lorsqu’aucun contenu ne s’applique, utiliser explicitement un tableau vide, jamais null. Chaque catégorie d’id n’est unique qu’au sein de son tableau ; les positions de référence sont interprétées explicitement selon cette annexe.

## 2. Structure de data de l’extension

| Champ | Type | Usage et valeur par défaut |
| --- | --- | --- |
| responsibility | Responsibility | Règles de décision du poste ou de l’ensemble |
| resources | Resource[] | Catalogue de méthodes, matériaux, applications et politiques ; peut être vide, et l’inscription n’impose pas une activation globale |
| works | Work[] non vide | Points d’entrée de travail promis à l’extérieur |
| requires | LocalId[], facultatif | resources nécessaires à tous les travaux ; omission équivalente à vide |
| routines | Routine[], facultatif | Organisations de travaux récurrents ; omission équivalente à vide, sans activation automatique |
| authorityRoles | AuthorityRole[], facultatif | Rôles externes d’approbation, de réception ou de responsabilité ; omission équivalente à vide |
| memoryPolicyRef | LocalId, facultatif | Référence une ressource purpose=policy ; compte dans les dépendances globales si présent ; omission signifie absence de déclaration de mémoire entre tâches |
| collaboration | Collaboration | Obligatoire pour organization, interdit pour agent |
| governance | Governance, facultatif | Uniquement pour organization ; obligatoire pour déclarer une définition AI org de cette version |

**Responsibility** contient cinq tableaux de chaînes obligatoires : `triggers`, `autonomousActions`, `confirmationPoints`, `escalationRules`, `deliveryChecks`. Ils décrivent respectivement quand prendre le travail, les actions autonomes, les points de confirmation, les règles d’escalade et les contrôles de livraison. triggers, autonomousActions et deliveryChecks doivent être non vides. Les deux autres peuvent être vides s’il n’existe réellement aucune confirmation métier ni règle d’escalade supplémentaire. Un tableau de confirmation vide ne dispense pas de l’autorisation de l’hôte ; un tableau d’escalade vide ne permet pas de continuer sans entrées ou permissions. Une description n’accorde aucun droit sur les outils. Les instructions du noyau décrivent la responsabilité durable ; Work et les ressources fournissent les détails. La substance du texte exige encore une révision de contenu et ne se déduit pas de chaînes simplement non vides.

**Resource** contient `id:LocalId`, `title:chaîne non vide`, `purpose`, `binding`, et facultativement `requires:LocalId[]`. purpose prend une valeur parmi `method | knowledge | procedure | application | runtime | policy | trigger | template-reference`. Ce sont des étiquettes d’usage de ressources, pas de nouveaux types de ressources produit obligatoires. binding contient `kind` et `config`, reprend les quatre liaisons de Capability du noyau et leurs règles config, sans redéfinir les protocoles.

Resource.requires référence uniquement les resources de la même définition ; l’omission équivaut à vide. Les matériaux fixes devraient de préférence porter un digest ; les sources variables doivent être fixées à la version lue lors de l’exécution réelle. Une méthode peut référencer un paquet complet de Skills ; une SOP peut référencer une ressource de méthode ou de procédure. Une simple description lisible d’application, configuration de modèle ou politique ne peut être déclarée comme application connectée, environnement satisfait ou politique appliquée.

Pour runtime, policy, trigger ou une application décrite sous forme de ressource, le rapport d’adaptation doit indiquer format natif, version exacte et responsable explicite du traitement. Celui-ci peut être une fonction de l’hôte prenant ce format en charge ou une étape humaine expressément confiée. Un simple texte sans responsable de sa mise en œuvre ne satisfait pas une condition nécessaire. Cette convention conserve les formats natifs existants et n’invente pas de langage général de permissions ou de planification.

**AuthorityRole** contient `id:LocalId`, `name:chaîne non vide`, `kind:human | service`. C’est un emplacement de responsabilité, pas un compte. L’hôte doit y lier un acteur fiable. service désigne un acteur de service externe confirmé par l’hôte, et non un modèle pouvant se proclamer approbateur. Si l’approbation humaine est nécessaire, human doit être déclaré. La fonction du rôle est déterminée par le contrat de réception, d’escalade ou de gouvernance qui le référence.

## 3. Entrées, sorties et livraison de chaque travail

| Champ Work | Type | Signification |
| --- | --- | --- |
| id, title, instructions | LocalId, chaîne non vide, chaîne non vide | Point d’entrée, nom, exigences de but/méthode |
| inputs, outputs | Objets JSON Schema | Décrivent respectivement l’objet d’entrée complet et l’objet de sortie complet |
| requires | LocalId[] | resources nécessaires à ce travail ; peut être vide |
| responsible | ParticipantRef | Une position responsable principale |
| acceptance | Acceptance | Vérification d’achèvement et responsabilité de réception |

`ParticipantRef` est `self`, `member/<LocalId>` ou `authority/<LocalId>`. self désigne l’instance correspondant à la définition courante. member ne désigne qu’un emplacement Member direct de l’organization courante, y compris un consultant participant à la tâche. authority désigne data.authorityRoles courant. agent ne doit pas utiliser de référence member. Toute référence doit exister, sans la deviner à partir d’un nom. responsible doit être self ou member. Les personnes externes peuvent prendre en charge réception et escalade, mais ne doivent pas être présentées comme exécutants IA. Pour organization, self signifie que l’ensemble est responsable de la livraison externe. L’hôte doit néanmoins enregistrer les exécutants réels et les responsables de la distribution des entrées et de l’agrégation des sorties ; l’identité de l’organisation ne remplace pas les contributeurs réels.

**Acceptance** contient `criteria:tableau non vide de chaînes`, `reviewer:ParticipantRef`, `independent:boolean`. criteria définit des exigences d’achèvement vérifiables ; reviewer est la position chargée de la réception. Avec independent=true, l’hôte doit prouver que le réviseur réel diffère de l’exécutant réel du résultat examiné. Pour un résultat composé, il doit vérifier les contributeurs réels, pas seulement le responsable principal ou l’identité nominale de l’équipe. Deux emplacements, deux sessions ou deux noms ne prouvent pas l’indépendance. Un acteur indépendant ne prouve pas automatiquement la qualité du jugement. L’enregistrement de réception fixe la version du résultat utilisée ; validation structurelle, achèvement du modèle et adoption métier sont enregistrés séparément.

La racine de inputs et outputs doit être `type:object` et déclarer `$schema = https://json-schema.org/draft/2020-12/schema`. Le vocabulaire standard de [JSON Schema 2020-12](https://json-schema.org/draft/2020-12) est réutilisé, sans langage de types propriétaire. Un Schema peut inclure des descriptions et `$defs`. Toutes les références, y compris `$ref` et `$dynamicRef`, doivent rester dans ce document Schema ; un `$id` imbriqué ne doit pas modifier la frontière documentaire des références. L’import ne résout pas les références par le réseau. Une implémentation doit signaler les vocabulaires ou contraintes non pris en charge ; elle ne doit pas les ignorer puis annoncer une validation réussie. Si format sert à une exigence essentielle de réception, la capacité de vérification de format correspondante doit être explicitement déclarée et vérifiée.

Une entrée manquante est signalée comme à compléter. Une sortie manquante ou de mauvais type interdit de déclarer la conformité au contrat de résultat. Aucun passage implicite de chaîne à nombre, aucune troncature des décimales et aucune conversion de false en 0 n’est permis. Schema default est descriptif et ne signifie pas automatiquement que l’utilisateur a fourni ou autorisé cette valeur. Un résultat libre peut se trouver dans un champ chaîne ; les preuves métier et la qualité du jugement restent contrôlées par acceptance.

**Vérification des dépendances :** satisfaire d’abord les capabilities du noyau et leurs conditions R05/R06 ; former ensuite l’union de data.requires, memoryPolicyRef et Work.requires du travail sélectionné, puis développer récursivement Resource.requires. Une référence absente ou un cycle de dépendances de ressources est une erreur de définition. Une connexion, une permission ou un interpréteur manquant rend le travail concerné non prêt ; les ressources spécifiques d’autres travaux non configurées ne bloquent pas le travail courant. Une dépendance globale nécessaire absente bloque toujours l’ensemble de l’exécution. Prise en charge de l’interprétation, configuration prête et autorisation réelle sont rapportées séparément ; une disponibilité partielle ne peut être déclarée comme exécutabilité totale.

## 4. Routines et expérience

**Routine** contient `id:LocalId`, `work:LocalId`, `policyRef:LocalId`, et facultativement `triggerRef:LocalId`. work référence les works courants, policyRef une ressource purpose=policy et triggerRef une ressource purpose=trigger. Le rôle d’exécution appartient à l’instance liée à la définition courante. Créateur, propriétaire de la planification et acteur réel de l’autorisation sont liés séparément par l’hôte et ne doivent pas nécessairement coïncider avec l’identité du Worker. L’import ne doit ni créer un autre employé, ni étendre la visibilité de la routine, ni la transférer à un autre membre.

L’absence de triggerRef signifie « à planifier ». Sa présence ne définit encore qu’un déclencheur ; l’instance reste désactivée par défaut. L’hôte traite le format du déclencheur. Les mécanismes daily/weekly ou événements locaux existants de Teloa ne déclarent que leur prise en charge réelle ; des cron arbitraires ou événements externes ne doivent pas être approximés silencieusement. Vérifier d’abord le travail et le déclencheur réels, puis laisser le déployeur activer.

La policy d’une routine doit préciser fenêtre/curseur d’entrée, comportement en absence d’entrée ou avec entrée périmée, déduplication des déclenchements et suivi du traitement métier, nouvelles tentatives, concurrence/rattrapage, budget, notifications et conditions de suspension. Un plan incomplet peut être conservé, mais ne doit pas être activé. Un déclenchement réussi ne signifie pas un traitement réussi. L’état de traitement métier ne progresse que lorsque l’état de résultat convenu est atteint ; une nouvelle tentative ne doit pas contourner la déduplication métier en créant un nouvel identifiant de déclenchement. Il n’existe ni nombre universel fixe de tentatives ni règle sectorielle « deux fois sans progrès ».

memoryPolicyRef définit seulement les règles de conservation, provenance, portée, révision et retrait de l’expérience ; il ne transporte pas la mémoire privée de l’auteur. Une ressource template-reference est un matériau de modèle avec provenance ; des journaux historiques ne peuvent être présentés comme des faits vécus par une nouvelle instance. Les contextes personnels, métier, de projet et de groupe d’un employé partagé restent séparés selon les autorisations de l’hôte. Une même instance dans plusieurs équipes ne rend pas leurs historiques mutuellement lisibles.

## 5. Collaboration d’équipe et gouvernance organisationnelle

| Champ Collaboration | Type | Signification |
| --- | --- | --- |
| roles | RoleAssignment[] | Couvre exactement une fois chaque Member direct |
| coordinator | ParticipantRef, facultatif | S’il est fourni, ne peut viser qu’un emplacement Member direct courant ; l’omission ne choisit personne automatiquement |
| handoffs | Handoff[] | Transmissions explicites de responsabilité et de matériaux ; peut être vide |
| sharing | Sharing[] | Positions destinataires souhaitées des ressources partagées ; peut être vide |
| stop | StopPolicy | Règles de collaboration bornée, absence de progrès et escalade |

**RoleAssignment** contient `member:LocalId`, `duty:chaîne non vide`, `participation:member | consultant`. duty décrit la répartition dans cet ensemble sans réécrire les responsabilités initiales du membre ; en cas de conflit, le travail concerné est arrêté. consultant désigne un conseiller participant à une tâche, sans rejoindre automatiquement un groupe ni obtenir son historique. Les références fixes des deux types d’emplacements sont résolues selon les règles du noyau ; le périmètre de participation est appliqué par la liaison d’instance et l’autorisation de tâche.

coordinator et Work.responsible peuvent viser un emplacement consultant, mais exigent une mission explicite et une confirmation de participation au travail courant. La participation reste limitée à la tâche ; elle ne confère ni direction permanente de l’équipe ni accès à tout l’historique du groupe. `member/` est la syntaxe de référence à un emplacement Member, distincte de la valeur member de participation.

**Handoff** contient `id:LocalId`, `from:WorkRef`, `to:WorkRef`, `when:chaîne non vide`, `inputs:objet`. WorkRef n’autorise que `member/<memberId>/<workId>`, référence à un travail déclaré d’un membre direct de l’ensemble courant ; sa version de définition provient de Member.ref. Il désigne la transmission d’un travail membre ayant produit un résultat vers le travail membre destinataire. `self/<workId>` n’est pas pris en charge ; entrées/sorties de l’ensemble ne sont pas mélangées à cette même sémantique de référence. Une sous-équipe expose ses propres works, sans accès direct à ses membres internes.

Les clés de Handoff.inputs sont les noms de champs de premier niveau de l’objet d’entrée du travail destinataire ; les valeurs sont les noms de champs de premier niveau de l’objet de sortie du travail expéditeur. Toutes sont interprétées comme chaînes originales, sans expression, script ou chemin à points. Les champs doivent être déclarés dans les Schema.properties correspondants ; les valeurs copiées doivent aussi passer la validation réelle du Schema destinataire. Les autres entrées nécessaires au destinataire doivent être explicitement fournies ; si elles manquent, les lacunes sont signalées sans simuler une réception réussie. when est une condition métier : la transmission n’a lieu que sur mission explicite ou confirmation par un mécanisme de déclenchement pris en charge ; une phrase ne prouve pas une surveillance mise en œuvre.

La distribution des entrées et l’agrégation des sorties de Work d’un ensemble relèvent de l’orchestration native de l’hôte ou d’un coordinateur expressément chargé de cette tâche. Cette version n’introduit pas de langage supplémentaire de correspondance entrée/sortie. Le rapport d’adaptation doit préciser quelle entrée de l’ensemble va à quel travail membre, qui fournit chaque sortie nécessaire et à partir de quel résultat fixe, et qui contrôle la complétude. Une validation statique de définition ne prouve pas l’exécution de ces correspondances. Tant qu’elles ou leur responsable ne sont pas déterminés, le travail de l’ensemble ne doit pas être marqué prêt. Le résultat composé doit satisfaire son Schema outputs et acceptance ; des sorties arbitraires de membres ne peuvent être assemblées automatiquement en résultat d’équipe.

Chaque transmission réelle fixe travail/Run source, version du résultat, acteurs expéditeur et destinataire, matériaux après correspondance, périmètre autorisé et résultat d’acceptation/refus. Elle n’est achevée qu’après confirmation du destinataire et établissement de l’attribution du travail. Seuls les matériaux nécessaires au contrat et autorisés sont transmis, sans transfert implicite de session entière, mémoire privée ou identifiants de connexion. L’envoi d’un message au groupe et la réussite d’une transmission sont comptabilisés séparément.

**Sharing** contient `resource:LocalId`, `recipients:tableau ParticipantRef non vide`. resource référence les resources courants ; les destinataires ne peuvent être que les member courants. Visibilité et version précise restent soumises à l’autorisation de l’hôte. sharing n’accorde aucune permission et ne transforme pas automatiquement une ressource partagée en dépendance obligatoire de tous les membres. Si un travail en a besoin, ses entrées ou dépendances doivent la prendre en compte explicitement.

**StopPolicy** contient `maxHops:entier positif`, `maxNoProgress:entier positif`, `escalateTo:ParticipantRef`. Les compteurs portent sur un même travail et sa chaîne de transmissions dérivées ; ils ne sont pas remis à zéro par changement d’employé ou nouvelle tentative. maxHops limite les transmissions ; maxNoProgress limite les cycles consécutifs sans nouvelle preuve vérifiable ni amélioration d’un critère de réception déclaré. Dès qu’une limite est atteinte, les affectations suivantes sont arrêtées et le problème est escaladé. La reprise exige un traitement explicite par un acteur fiable et un nouvel enregistrement. Les valeurs sont configurées selon le scénario et ne sont pas des scores de capacité des employés.

**Governance** contient `accountable:ParticipantRef`, `changePolicyRef:LocalId`. accountable est la position responsable du périmètre organisationnel. changePolicyRef référence une ressource purpose=policy et compte automatiquement parmi les dépendances globales. Cette politique doit préciser nomination/retrait des membres, changements de définitions et contraintes, suspension et traitement du travail en cours. Les limites organisationnelles de données, budget et exécution sont exprimées par les ressources policy/runtime de data.requires. Toute contrainte à hériter doit nommer explicitement membres concernés et portée, puis être vérifiée par l’hôte. Les contraintes n’étendent pas automatiquement les permissions des membres ; un conflit ne doit pas être résolu par la règle la plus permissive.

Une AI Team utilise un contrat de collaboration ; une définition AI Organization de cette version exige aussi governance. Toutes deux utilisent le même type de noyau organization, sans distinction par effectif. De nouvelles permissions organisationnelles, une gestion réelle des membres ou un système d’identité interorganisations ne sont pas automatiquement établis par cette spécification de définition.

Une déclaration de conformité complète Team ou Organization exige que tous les agent de la fermeture transitive des références de membres satisfassent le contrat de définition AI Worker et que toutes les organization fournissent le contrat de collaboration de cette annexe. Il n’est pas nécessaire que chaque sous-équipe déclare governance. Un Agent temporaire ne possédant que les champs du noyau peut être appelé sous autorisation par le mécanisme d’exécution d’un Worker, mais ne peut passer pour un membre durable conforme à cette version. Les positions externes de réception humaine ou de service sont exprimées par authorityRoles.

## 6. Liaisons de l’hôte et exécution réelle

Les liaisons sont enregistrées dans l’environnement de déploiement. Tous les hôtes ne doivent pas utiliser la même base ou structure JSON, mais les informations suivantes doivent pouvoir être vérifiées :

| Enregistrement | Contenu à distinguer obligatoirement |
| --- | --- |
| Position de définition | id/revision de la racine, chemin d’emplacements composé de Member.id successifs, définition fixe à l’extrémité |
| Choix d’instance | Création ou réutilisation explicite, identité/version réelle, acteur fiable de rattachement ; aucune création lors de l’import |
| Continuité d’identité | Instance traçable entre tâches ; identités de modèle, session, Agent temporaire et Run enregistrées séparément ; activation, suspension, retrait et autorisation de reprise vérifiables |
| Acteurs de tâche et d’autorisation | Gestionnaire, initiateur de tâche, propriétaire de routine et autorisateur des ressources enregistrés séparément ; partager un Worker ne signifie pas partager toutes les autorisations d’une personne |
| Relation de réutilisation | Plusieurs emplacements liés ou non à la même instance ; cette version exige la même définition fixe pour un partage, sans inférence fondée sur le nom |
| Rôles de responsabilité | Acteurs fiables et types human/service correspondant à authorityRoles ; l’approbateur ne peut provenir d’une identité autoaffirmée par le modèle |
| Conditions d’exécution | Travail sélectionné, ressources, formats/versions natifs, connexions réelles, autorisation valide, modèle et limites de ressources |
| Versions et changements | Version des liaisons, répartition des membres retenue, versions des contraintes et matériaux, traitement des travaux en attente après changement |

Deux instances issues du même modèle et une même instance affectée à plusieurs équipes doivent produire des enregistrements différents. Visibilité des groupes et autorisation d’exécution de l’instance partagée sont vérifiées par travail. Le budget est comptabilisé sur le même quota réellement applicable ; multiplier les liaisons ne doit pas transformer un quota unique en deux quotas.

Une exécution doit être traçable jusqu’à la définition racine, au travail sélectionné, à l’exécutant réel, aux versions de matériaux et ressources, à l’autorisation valide et à la version du résultat. Remplacer un membre ou mettre à jour un modèle ne reprend pas silencieusement un ancien Run. Reprise, arrêt ou nouveau travail suivent une politique explicite de changement. Si l’hôte ne peut respecter une contrainte essentielle, il conserve la définition et signale le blocage, au lieu de la réduire à une remarque et de continuer.

Entrées de travail, matériaux récupérés, messages historiques et sorties d’outils sont des matériaux de travail avec provenance, pas des instructions fiables permettant de modifier la définition du Worker ou d’étendre ses permissions. L’hôte doit les distinguer des sources d’autorisation. Un Worker peut appeler des Agents temporaires, mais la livraison externe reste attribuée au Worker choisi ; ces exécutants ne deviennent pas automatiquement membres d’une Team. La révision indépendante examine les contributeurs réels et relations de délégation ; une nouvelle session ou un assistant temporaire ne doit pas créer artificiellement une identité indépendante pour contrôler ses propres résultats.

Les traces d’exécution doivent au moins distinguer réussite, échec, réalisation partielle, résultat inconnu et suspension, même si l’hôte utilise ses propres noms d’état. Des sorties partielles ou une réponse après expiration du délai ne peuvent être marquées automatiquement comme réussite. En cas d’effet externe dont le résultat est inconnu, le statut de l’opération initiale doit être vérifié avant de décider d’une nouvelle tentative. Exigences de tâche, conditions de réception, résultat et limites constituent le périmètre d’évaluation de qualité ; aucun score commun à tous les secteurs ni principe « une réussite suffit pour entrer en fonction » n’est imposé.

## 7. Limites réelles de la reprise d’OASF et d’Agent Spec

OASF discovery est porté par une extension distincte : l’URI du Schema discovery natif dans une version fixe sert d’id, la même schema_version sert de version, et le payload natif est conservé tel quel dans data. Pour de simples informations de découverte, required=false. S’il sert à une condition de sélection d’exécution non ignorable, required=true doit être déclaré et sa sémantique validée ; la classification n’accorde toujours aucune permission. Ce wrapper est la méthode de correspondance de cette spécification, pas un corps de requête A2A natif.

Par exemple, `schema_version`, `skills` et `domains` du payload natif conservent leurs noms et sens. skills représente des catégories de capacités, domains des domaines. Seules les entrées réellement présentes dans le catalogue fixe peuvent être déclarées comme ayant passé la validation de classification OASF. L’hôte peut utiliser hors ligne des instantanés fiables et fixes de Schema/classification ; importer une URI n’entraîne pas automatiquement un accès au service. En l’absence de classification correspondante, conserver une note de non-correspondance sans inventer d’identifiant officiel. [OASF discovery](https://docs.agntcy.org/oasf/discovery-extension/)

Les métadonnées record et locators d’OASF peuvent servir de vue d’export ; source, version et champs non mappés figurent dans le rapport d’adaptation. Ils ne remplacent ni les révisions fixes de cette spécification ni les identités d’instance réelles. La définition source reste la seule référence ; une vue dérivée ne devient pas une deuxième configuration d’exécution indépendamment modifiable. [Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Les idées d’Agent Spec concernant références, entrées/sorties et composition sont reprises dans Work et Handoff. `$component_ref` est une référence interne à une configuration ; l’export doit établir explicitement sa relation avec les définitions/ressources fixes de cette spécification. Ses listes de propriétés d’entrée/sortie et les Schemas d’objet de cette version nécessitent une conversion, pas un simple renommage. La prise en charge de la correspondance d’une version ou d’un sous-ensemble choisi ne peut être déclarée que si interfaces, contraintes et sémantique d’exécution sont entièrement préservées. [Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

Swarm et ManagerWorkers peuvent être des cibles d’adaptation pour des équipes correspondantes ; transfert de session, possibilité pour les membres d’interagir directement avec l’utilisateur et flux de messages doivent être vérifiés séparément. Le contrat actuel n’impose ni le passage de Teloa à leur moteur d’exécution ni la conversion de toutes les équipes vers un modèle superviseur–travailleurs.

## 8. Couverture des conceptions existantes

| Élément existant | Destination dans cette spécification |
| --- | --- |
| A01 Identité du poste et mission | id/revision/name/instructions du noyau ; données d’affichage telles qu’avatar conservées dans une extension non essentielle |
| A02 Responsabilité et jugement | Cinq groupes de champs responsibility ; autorisation réelle dans la liaison de l’hôte |
| A03 Skills | resources purpose=method, référence à un paquet complet de méthodes ou une ressource native de méthode |
| A04 Connaissances | purpose=knowledge, conservation de source, version ou digest, référence par travail |
| A05 SOP | purpose=procedure et dépendances de ressources ; transmissions entre rôles exprimées par collaboration |
| A06 Modèles de travail | But, I/O, dépendances, position responsable et réception de works |
| A07 Routines | routines, déclencheurs natifs et politiques d’exécution ; états non configuré/non activé enregistrés par l’hôte |
| A08 Connexions applicatives | purpose=application, liaison native et connexion réelle ; déclaration au catalogue ne prouve pas une installation réussie |
| A09 Permissions | Ressources policy nécessaires, périmètre de travail, autorisation fiable de l’hôte ; aucun export d’identifiants secrets |
| A10 Exécution et espace | Exigences de ressources runtime et liaisons de modèle, budget et périmètre de fichiers de l’hôte |
| A11 Expérience | memoryPolicyRef et références de provenance ; expérience privée réelle conservée dans l’instance |
| A12 Résultats et qualité | outputs, acceptance et preuves d’exécution ; structure et qualité métier vérifiées séparément |
| T01 Objectif commun | organization.instructions et works exposés à l’extérieur |
| T02 Responsabilités des postes | Member, collaboration.roles, Work.responsible et authorityRoles |
| T03 Contexte partagé | sharing, entrées de travail et autorisation par périmètre de l’hôte |
| T04 Transmissions collaboratives | handoffs, correspondance des entrées, confirmation de réception et stop |
| T05 Organisation du travail | routines et procedure des membres ou de l’ensemble, répartition de la coordination |
| T06 Permissions et ressources | requires globaux/par travail, droits initiaux des membres et quotas réels de l’hôte |
| T07 Réception de l’équipe | outputs/acceptance de l’ensemble, contributions des membres et versions fixes des résultats |

RoleDefinition, GroupDefinition, RoleWorkDelegation, RunRoleSnapshot et les relations de paquets de ressources du dépôt communautaire courant servent d’objets d’adaptation, sans devenir de nouveaux objets produit. L’« employé IA » de Teloa correspond à la forme produit du Worker. RoleDefinition, version fixe de poste et instance Role réelle participent ensemble à la correspondance ; renommer seul ne prouve pas la conformité. role-coordination n’exprime qu’une relation statique de coordination ; création de groupe, affectation et transmission automatique exigent chacune une prise en charge et une autorisation réelles.

Le `kind: employee | twin` de Teloa représente un type de rôle produit. Les deux peuvent être mappés comme Worker s’ils remplissent le même contrat ; twin n’accorde aucun droit d’approbation au nom de la personne. Le kind d’origine est conservé dans le rapport d’adaptation ou une extension d’affichage non essentielle, jamais directement dans kind du noyau. `scopes`, avec dataScope/executionScope, est mappé vers des politiques de périmètre nécessaires et des liaisons d’autorisation de l’hôte. Si un périmètre métier ne peut être mis en correspondance entre hôtes, le travail concerné reste non prêt. Ce sont des restrictions essentielles qui ne peuvent être stockées uniquement dans une extension d’affichage ignorable. Ces champs ont été comparés à `packages/contract/src/roles.ts` du dépôt communautaire ; cela ne signifie pas qu’un convertisseur est déjà implémenté.

Dans les recipes publiques de Grok, profile correspond à la description de poste, skills aux méthodes, routines aux travaux et routines à planifier, plugins aux applications à configurer, memory aux matériaux de modèle avec provenance. Les consignes de travail de type profile et journaux d’auteur de type log sont préservés séparément. gettingStarted reste un simple conseil d’entrée. Les informations manquantes sur responsabilités, calendrier ou réception sont signalées, sans invention fondée sur le nom ou l’expérience de l’auteur initial. L’original complet et son digest sont liés par le rapport d’adaptation ; une sémantique essentielle ne doit pas être cachée dans une extension ignorable. Grok Team Bot est mappé vers un Worker partagé, sans générer automatiquement une Team à plusieurs Workers d’après son nom.

La documentation publique de Grok Team Bot attribue les routines personnelles à leur créateur ; les connexions peuvent aussi fonctionner avec les identifiants de l’utilisateur courant ou du Bot. L’adaptation doit donc préserver propriétaire de routine, identité d’exécution et visibilité des matériaux, sans fusionner les routines de tous en tâches communes à tous les utilisateurs du Worker partagé. C’est une exigence de correspondance du comportement source, pas l’obligation pour d’autres hôtes de copier ses permissions par défaut. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

**Destination des routines privées :** une routine personnelle est uniquement mappée vers une organisation d’exécution côté hôte, isolée par propriétaire et visibilité, qui référence un Work déclaré du Worker. Elle ne doit ni entrer dans routines de la définition partagée ni être exportée avec elle. Le rapport porte la mention « conservée comme organisation privée d’instance, non incluse dans la définition » et doit lui-même respecter cette visibilité. Une routine ne peut entrer dans routines comme modèle qu’après publication explicite par l’auteur original et retrait du contenu privé. L’import reste désactivé par défaut. S’il est impossible de conserver propriétaire, visibilité ou référence au travail, signaler l’impossibilité de correspondance ; aucune fusion ni perte silencieuse n’est permise.

Le rapport d’adaptation indique au minimum format/version/digest source, révision de définition cible, correspondances de champs, éléments conservés/convertis/non pris en charge et travaux affectés. Conserver l’original ne signifie pas réussir l’adaptation sémantique. Une conversion incapable de respecter permissions, transmissions ou conditions de réception doit bloquer les travaux concernés.

## 9. Cas positifs et négatifs et déclarations d’achèvement

L’[exemple complet de composition](../examples/composition-fixture.json) utilise un ensemble local fixe de définitions : deux employés, deux équipes réutilisant leurs deux définitions, une organisation imbriquée, ainsi que les attentes de liaison d’hôte et de disponibilité du travail. L’enveloppe et les explications sont un format de fixture de test, pas un nouveau protocole de distribution ni un paquet directement importable dans Teloa.

| Vérification | Conclusion obligatoire |
| --- | --- |
| Poste simple sans Skills, mémoire ou routines | Définissable ; travail, I/O, responsabilité et réception explicites restent obligatoires |
| Seulement un nom, un prompt et kind=agent | Peut satisfaire l’échange du noyau ; ne permet pas de déclarer une définition AI Worker complète |
| triggers, actions autonomes ou contrôles de livraison vides | Non conforme à cette annexe ; le nom du produit ne complète pas les lacunes |
| Même Worker exécutant deux tâches et changeant de modèle | Même instance de Worker et deux Runs distincts doivent être traçables ; deux sessions seules ne prouvent pas la continuité d’identité |
| Même définition d’employé dans deux équipes | Montre seulement une réutilisation de définition ; la liaison des emplacements détermine si l’instance est identique |
| Un travail manque d’application dédiée, un autre possède toutes ses entrées | Le premier n’est pas prêt ; le second vérifie encore conditions globales et dépendances propres et n’est pas automatiquement bloqué par le premier |
| Version de sous-organisation absente, cycle ou profil obligatoire inconnu | Arrêter l’assemblage et signaler la référence précise, sans changer de version ou ignorer |
| Mauvais champ destinataire, entrée obligatoire manquante ou mauvais type de sortie | Ne pas déclarer transmission réussie ou résultat conforme ; aucune complétion ni conversion de type automatique |
| Deux emplacements de révision liés au même exécutant réel | independent=true non satisfait |
| Consultant participant seulement à une révision | Aucun ajout automatique à tous les groupes, accès à tout l’historique ou copie d’un nouvel employé |
| Déclencheur absent ou politique incomplète | Conserver comme plan non prêt, sans activer |
| Classification OASF inconnue ou modèle Agent Spec non équivalent | Signaler non-prise en charge/différence de correspondance, sans déclarer une compatibilité complète |

Cette spécification déclare des contrats structurels et comportementaux. Vérification du texte, vérification des exemples, connexion/exécution de modèle réelles, interopérabilité d’implémentations indépendantes et adoption sectorielle sont rapportées séparément. La portée de validation et les exigences de rapport d’implémentation figurent dans les [limites de conformité](../CONFORMANCE.md).
