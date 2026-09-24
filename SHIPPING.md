# Comment Alba arrive dans la main des gens

Une seule question : ils l'ouvrent où, et combien de temps ça coûte.

## La décision

**Deux étapes, pas deux produits.**

1. **Maintenant — PWA.** Le site web, installable. Le lien SMS l'ouvre, et le
   téléphone propose de l'ajouter à l'écran d'accueil. Zéro store, zéro délai,
   une seule base de code. C'est ce qui part chez le premier hôtel.
2. **Ensuite — Capacitor.** Le **même** code emballé en vraie app iOS et Android,
   déposée sur l'App Store et le Play Store. Rien à réécrire.

**Ce qu'on ne fait pas : une app native séparée** (Swift / React Native). Ce serait
un deuxième codebase à maintenir pour zéro client de plus aujourd'hui. On y
reviendra si un jour une limite technique l'impose — elle n'existe pas encore.

## Pourquoi pas juste la PWA

La PWA suffit pour tout, **sauf une chose** : sur iPhone, les notifications ne
marchent que si la personne a ajouté l'app à son écran d'accueil (iOS 16.4+).

Or la notification n'est pas une option dans Alba — c'est le produit. « La cuisine
est prête », « Emeline a répondu 11:30 », « Technique n'a pas ouvert ». Si un
serveur sur trois n'a pas fait « Ajouter à l'écran d'accueil », le produit ne tient
pas sa promesse pour lui, et on ne le saura pas.

C'est la seule raison d'aller sur les stores. Pas le prestige : la fiabilité de la
notification.

## Ce que ça coûte

| | Prix | Délai |
|---|---|---|
| PWA | 0 € | déjà faisable |
| Compte développeur Apple | 99 €/an | 24–48 h à ouvrir |
| Compte Google Play | 25 € une fois | quelques heures |
| Première revue App Store | — | 1 à 3 jours en général |
| Mise en forme (icônes, écrans, fiches, confidentialité) | 0 € | 1 à 2 jours de travail |

Compter **une à deux semaines** entre « on décide » et « c'est sur l'App Store »,
dont l'essentiel est de la paperasse, pas du code.

## Ce que Capacitor apporte en plus du web

- Notifications push natives (APNs / FCM) — celles qui sonnent vraiment
- Appareil photo natif pour photographier le BEO, plus net et plus rapide
- L'app reste ouverte en tâche de fond pendant un service
- L'icône sur l'écran d'accueil sans expliquer à personne comment l'y mettre
- Une fiche sur l'App Store — ce qui compte devant un directeur : ça existe

## L'onboarding, dans cet ordre

C'est ce que dessinent les trois écrans « Mise en route » sur le canvas.

1. Le directeur crée l'établissement : nom, type, adresse → un identifiant
   (`ALBA-7741`) qui relie BEO, équipes et rapports.
2. Les six départements existent déjà : Commercial, Banquet, Cuisine, Restaurant,
   Technique, Réception.
3. Il invite par **téléphone ou e-mail** : prénom, département, rôle.
4. La personne reçoit un SMS avec un lien. **Pas de mot de passe à créer** — un
   commis n'en créera pas. Le lien ouvre Alba, l'installation se propose après.
5. Un département sans personne s'affiche vide, en pointillés. Alba ne fait jamais
   semblant d'avoir prévenu quelqu'un qui n'existe pas.

## Le sentiment iOS, sans copier iOS

Ce qui fait « familier » tient à cinq choses, et on en a déjà trois :

- **Chevron ‹ en haut à gauche** pour une page, **poignée ▬ en haut** pour une
  feuille qui glisse. Fait.
- **Cibles de 44 px minimum**, action principale en bas, sous le pouce. Fait.
- **Zones sûres** (encoche, barre du bas) respectées. Fait.
- **Barres translucides floutées** — le « glassy ». À ajouter : `backdrop-filter:
  saturate(180%) blur(20px)` sur la barre du haut et la barre d'onglets, fond
  blanc à ~72 %. Ça marche sur les deux plateformes.
- **Balayer depuis le bord gauche pour revenir.** Capacitor le donne ; en PWA il
  faut le coder.

Ce qu'on ne copie pas : les couleurs et la typo d'Apple. Alba garde son papier
chaud et sa Fraunces. On emprunte les **gestes**, pas le costume — c'est ce qui
fait qu'une app semble bien faite plutôt qu'imitée.

## Les quatre choses à faire, dans l'ordre

1. Manifest + service worker + icônes → la PWA s'installe. (1 jour)
2. Barres translucides + zones sûres + retour par balayage. (1 jour)
3. Compte Apple + compte Google ouverts — le délai administratif court pendant
   qu'on code. (à lancer **maintenant**, c'est gratuit d'attendre)
4. Capacitor + push natif + fiches store. (3 à 5 jours)

Le premier hôtel peut démarrer après l'étape 1.
