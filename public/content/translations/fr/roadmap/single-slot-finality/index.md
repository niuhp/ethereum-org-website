---
title: Finalité à créneau unique
description: Tout savoir sur le protocole de Finalité à créneau unique
lang: fr
---

# Finalité à créneau unique {#single-slot-finality}

Cela demande à peu près 15 minutes pour qu’un bloc Ethereum soit finalisé. Cependant, nous pouvons faire en sorte que le mécanisme de consensus d'Ethereum valide les blocs plus efficacement et réduise considérablement le délai de finalisation. Plutôt que d'attendre quinze minutes, les blocs pourraient être proposés et finalisés dans le même créneau. Ce concept est connu** sous l'appellation de Single Slot Finality (SSF) ou Finalité à créneau unique.**

## Qu’est-ce que la finalité ? {#what-is-finality}

Dans le mécanisme de consensus basé sur la preuve d'enjeu d'Ethereum, la finalité fait référence à la garantie qu'un bloc ne peut pas être modifié ou supprimé de la blockchain sans que cela n'entraîne de l'action brûler (destruction) au moins 33 % de l'ETH total mis en jeu. Il s'agit d'une sécurité « crypto-économique » car la confiance provient du coût extrêmement élevé, associé à la modification de l'ordre ou du contenu de la chaîne, ce qui empêcherait tout acteur économique rationnel de tenter d'effectuer ce genre d'action.

## Pourquoi viser une finalité plus rapide ? {#why-aim-for-quicker-finality}

La durée actuelle de la finalité s'est avéré être trop longue. La plupart des utilisateurs ne désirent pas attendre 15 minutes pour le processus de finalité. C'est plutôt gênant pour les applications et autres plateformes d'échange qui ont plutôt besoin d'un débit de transaction élevé, mais doivent patienter tout ce temps afin d'être assurés que leurs transactions soient permanentes. Le délai entre la proposition d'un bloc et sa finalisation crée également une opportunité de courtes réorganisations qu'un attaquant pourrait utiliser pour censurer certains blocs ou extraire la Valeur maximale extractible (MEV). Le mécanisme qui gère la mise à niveau des blocs par étapes est également assez complexe et a été corrigé à plusieurs reprises pour corriger les vulnérabilités de sécurité, ce qui en fait l'une des parties de la base de code Ethereum où des bogues subtils sont les plus susceptibles de survenir. Ces dilemmes pourraient tous être éradiqués en réduisant le temps de finalité à un seul créneau.

## Le compromis décentralisation/temps/frais généraux {#the-decentralization-time-overhead-tradeoff}

La garantie de finalité n'est pas une propriété immédiate d'un nouveau bloc ; il faut du temps pour qu'un nouveau bloc se finalise. La raison en est que les validateurs représentant au moins 2/3 du total des ETH mis en jeu sur le réseau doivent voter pour le bloc (« attester ») afin qu'il soit considéré comme finalisé. Chaque nœud de validation du réseau doit traiter les attestations d'autres nœuds, dans le but de savoir si un bloc a ou n'a pas atteint ce fameux seuil des 2/3.

Plus le temps imparti pour atteindre la finalisation est court, plus la puissance de calcul est importante sur chaque nœud car le traitement de l'attestation doit être effectué plus rapidement. De plus, plus il y a de nœuds de validation sur le réseau, plus d'attestations doivent être traitées pour chaque bloc, ce qui augmente également la puissance de traitement requise. Plus la puissance de traitement requise est élevée, moins le nombre de personnes pouvant participer est élevé, car un matériel plus coûteux est nécessaire pour exécuter chaque nœud de validation. L'augmentation du temps entre les blocs réduit la puissance de calcul requise sur chaque nœud, mais allonge également le délai de finalisation, car les attestations sont traitées plus lentement.

Ainsi, il existe un compromis entre la surcharge (puissance de calcul), la décentralisation (nombre de nœuds pouvant participer à la validation de la chaîne) et le délai conduisant à la finalité. Ainsi, le système idéal équilibre la puissance de calcul minimale, la décentralisation maximale et le temps minimum menant à la finalité.

L'actuel mécanisme de consensus d'Ethereum a équilibré ces trois paramètres en :

- **Fixant la participation minimale à 32 ETH**. Cela fixe une limite supérieure au nombre d'attestations de validateurs qui doivent être traitées par des nœuds individuels, et donc une limite supérieure aux exigences de calcul pour chaque nœud.
- **Fixant le délai à la finalité à 15 minutes environ**. Cela donne suffisamment de temps aux validateurs exécutés sur des ordinateurs personnels normaux pour traiter en toute sécurité les attestations pour chaque bloc.

Avec l'actuelle conception du mécanisme, et dans l'espoir de réduire le délai à la finalité, il est donc impératif de réduire le nombre de validateurs sur le réseau, ou bien d'augmenter les exigences matérielles (hardware) pour chaque nœud. Cependant, des améliorations peuvent être apportées à la manière dont les attestations sont traitées, ce qui pourrait permettre de compter davantage d'attestations sans ajouter de surcharge à chaque nœud. C'est pourquoi le traitement le plus efficace permettra de déterminer la finalité à l'intérieur d'un seul créneau, plutôt qu'à travers deux périodes distinctes.

## Routes vers la finalité à créneau unique ou Single Slot Finality (SSF) {#routes-to-ssf}

<ExpandableCard title= "Pourquoi nous ne pouvons pas bénéficier aujourd'hui de la Finalité à créneau unique (SSF) ?" eventCategory="/roadmap/single-slot-finality" eventName="clicked Why can't we hear SSF today?">

Le mécanisme de consensus actuel combine les attestations de plusieurs validateurs, aussi appelés comités, et ce, pour réduire le nombre de messages que chaque validateur doit traiter pour valider un bloc. Chaque validateur a la possibilité d'attester à chaque période (32 créneaux), cependant, dans chaque créneau, seul un sous-ensemble de validateurs, appelé « comité », a le pouvoir d'attester. Ils agissent ainsi en se divisant en sous-réseaux parmi lesquels quelques validateurs sont sélectionnés pour devenir « agrégateurs ». Ces agrégateurs combinent chacun toutes les signatures qu'ils voient provenant d'autres validateurs de leur sous-réseau en une seule signature globale. L'agrégateur qui comprend le plus grand nombre de contributions individuelles transmet sa signature globale à l'auteur du bloc (proposant), et ce dernier l'inclut dans le bloc avec la signature agrégée des autres comités.

Ce processus procure une capacité suffisante pour que chaque validateur puisse voter à chaque période car : « 32 créneaux * 64 commités * 256 validateurs par comité = 524 288 validateurs par période ». Au moment de la rédaction du présent document (février 2023), il y a environ 513 000 validateurs actifs.

Concernant ce schéma, chaque validateur n'a la possibilité de voter sur un bloc, qu'en répartissant ses attestations sur l'ensemble de la période. Toutefois, il est possible d'améliorer le mécanisme de manière à ce que _chaque validateur, ait la possibilité d'attester dans chaque créneau_.
</ExpandableCard>

Depuis la conception du mécanisme de consensus Ethereum, le système d'agrégation des signatures (BLS) s'est révélé beaucoup plus évolutif qu'on n'eût pu l'imaginer au départ, tandis que la capacité des clients à traiter et vérifier les signatures s'est également améliorée. Il s'avère que le traitement des attestations d'un grand nombre de validateurs est en fait rendu possible à l'intérieur d'un seul et même créneau. Exemple : avec un million de validateurs votant chacun deux fois dans chaque créneau, et des créneaux horaires calibrés à 16 secondes, les nœuds seraient poussés à vérifier les signatures lors d'un rythme minimum de 125 000 agrégations par seconde, afin de traiter l'ensemble du million d'attestations à l'intérieur dudit créneau. En réalité, il faut environ 500 nanosecondes à un ordinateur normal pour vérifier une signature, entendant le fait que 125 000 signatures peuvent être vérifiées en environ 62,5 ms, soit bien en deçà du seuil d'une seconde.

Des gains d'efficacité supplémentaires pourraient être accomplis en créant des super-comités composés, par exemple, de 125 000 validateurs par créneau sélectionnés de manière aléatoire. Seuls ces validateurs pourraient voter sur un bloc et, par conséquent, uniquement ce sous-ensemble de validateurs déciderait, ou non, de la finalisation d'un bloc. Que ce soit une bonne idée ou non dépend du coût que la communauté préférerait pour une attaque réussie contre Ethereum. En effet, au lieu d'exiger 2/3 du total de l'ether mis en jeu, un attaquant pourrait finaliser un bloc illégitime avec 2/3 des Ethers (ETH) mis en jeu _parmi ce super-comité_. À l'heure actuelle, il s'agit encore d'un domaine de recherche actif, mais il semble plausible au premier abord, que pour un ensemble de validateurs suffisamment important pour nécessiter des super-comités, le coût de l'attaque d'un de ces sous-comités soit extrêmement élevé (p. ex. le coût de l'attaque exprimée en ETH serait de `2/3 * 125 000 * 32 = 2,6 millions d'ETH`). Le coût de l'attaque peut être ajusté en augmentant la taille de l'ensemble des validateurs (par exemple, ajuster la taille du validateur pour que le coût de l'attaque soit égal à 1 million d'éther, 4 millions d'éther, 10 millions d'éther, etc.). [Les sondages préliminaires](https://youtu.be/ojBgyFl6-v4?t=755) de la communauté semblent indiquer qu'un coût d'attaque de 1 à 2 millions d'éthers s'avère acceptable, ce qui implique entre 65 536 et 97 152 validateurs par super-comité.

Toutefois, la vérification n'est pas le véritable point d'engorgement - c'est bien l'agrégation des signatures qui constitue un véritable défi pour les nœuds de validation. Pour faire évoluer l'agrégation des signatures, il faudra probablement augmenter le nombre de validateurs à l'intérieur de chaque sous-réseau, accroître le nombre de sous-réseaux ou encore ajouter des couches supplémentaires d'agrégation (c'est-à-dire mettre en place des comités de comités). Une partie de la solution consisterait à autoriser des agrégateurs experts - similaire à la manière dont la construction de blocs et la génération d'engagements pour les données rollup seront sous-traitées à des constructeurs de blocs spécialisés dans le cadre de la séparation entre le proposant et le constructeur de bloc (PBS), ainsi que de la méthode Danksharding (technique qui permet de partitioner un ensemble de données venant d'une même base de données).

## Quel est le rôle de la règle de choix de fourche dans le cadre SSF (Finalité à créneau unique) ? {#role-of-the-fork-choice-rule}

Le mécanisme de consensus actuel repose sur un couplage étroit entre le dispositif de finalité (l'algorithme qui détermine si 2/3 des validateurs ont attesté d'une certaine chaîne) et la règle du choix de fourche (l'algorithme qui décide quelle chaîne est la plus appropriée lorsque plusieurs options sont possibles). L'algorithme de choix de fourche ne prend en compte que les blocs _depuis_ le dernier bloc finalisé. À l'intérieur du cadre de la Finalité à créneau unique (SSF), il n'y aurait aucun bloc à prendre en compte pour la règle de choix de fourche, car la finalité se produit dans le même créneau que celui où le bloc est proposé. Cela signifie qu'en vertu de la SSF, _soit_ l'algorithme de choix de fourche _soit_ le dispositif de finalité serait opérationnel à tout moment. Le dispositif de finalité finaliserait les blocs pour lesquels les deux tiers des validateurs étaient en ligne et attestaient honnêtement. Si un bloc n'est pas en mesure de surpasser le seuil des deux tiers, la règle du choix de fourche intervient pour déterminer la chaîne à suivre. Cela crée également une opportunité de maintenir le mécanisme de brèche d'inactivité qui rétablit une chaîne où >1/3 des validateurs sont hors-ligne, nonobstant quelques nuances supplémentaires.

## Questions non résolues {#outstanding-issues}

Le problème avec l'évolutivité de l'agrégation en augmentant le nombre de validateurs par sous-réseau, c'est que cette dernière induit une charge beaucoup plus conséquente sur le réseau peer-to-peer. Le problème avec l'ajout de couches d'agrégations est qu'il est assez complexe à concevoir et ajoute de la latence (c'est-à-dire que cela pourrait prendre plus de temps avant que le proposant de bloc ne reçoive les informations de tous les agrégateurs de sous-réseaux). Aussi, la façon d'aborder le cas n'est pas encore très claire lorsqu'il y a bien plus de validateurs actifs sur le réseau, qu'il n'est possible d'en traiter dans chaque créneau, et ce, même avec l'agrégation des signatures BLS. Une solution envisageable : étant donné que tous les validateurs attestent dans chaque créneau et qu'il n'y a pas de comités sous le régime de Finalité à créneau unique (SSF), le plafond de 32 ETH du solde effectif pourrait être entièrement supprimé, signifiant ainsi que les opérateurs gérant de nombreux validateurs, pourraient consolider leurs enjeux et en exploiter une quantité moindre, réduisant de facto le volume de messages que les nœuds de validation doivent traiter pour tenir compte de l'ensemble des validateurs. Cela repose sur l’acceptation par les grands stakers de consolider leurs validateurs. Il est également possible d'imposer à tout moment un seuil fixe au nombre de validateurs ou au montant d'ETH mis en jeu. Cependant, cela requiert un certain mécanisme pour décider quels validateurs sont autorisés à participer et ceux déboutés de cette possibilité, ce qui est susceptible d'engendrer des effets secondaires indésirables.

## Progrès actuels {#current-progress}

La Finalité à créneau unique (SSF) est en phase d'études. Celle-ci ne devrait pas être opérationnelle avant plusieurs années, probablement après que d'autres avancées substantielles aient été réalisées, telles que [l'Arbre de Verkle](/roadmap/verkle-trees/) et la [solution Danksharding](/roadmap/danksharding/).

## Complément d'information {#further-reading}

- [Vitalik sur la Finalité à créneau unique (SSF) au EDCON 2022](https://www.youtube.com/watch?v=nPgUKNPWXNI)
- [Notes de Vitalik : Chemins menant à la Finalité à créneau unique (SSF)](https://notes.ethereum.org/@vbuterin/single_slot_finality)