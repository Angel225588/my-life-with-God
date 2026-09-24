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

## Les comptes — les liens directs

| | Lien | Prix |
|---|---|---|
| Google Play Console | https://play.google.com/console/signup | 25 $ une fois |
| Apple Developer | https://developer.apple.com/programs/enroll/ | 99 €/an |
| Numéro D-U-N-S (gratuit) | https://developer.apple.com/enroll/duns-lookup/ | 0 € |

La page Google ne se trouve pas depuis le Play Store — il faut y aller par ce lien.

### Décision prise : individuel, pour l'instant

Alba tourne sous une **micro-entreprise**. Apple ne l'accepte pas comme
« organisation » (entreprise individuelle, pas personne morale). Donc :

- **Apple** : inscription **Individual**, nom légal exact de la pièce d'identité.
  Pas de D-U-N-S.
- **Google** : compte **Personal**. La règle des 12 testeurs / 14 jours est un non-
  problème ici : on a une équipe hôtelière qui utilise l'app tous les jours. Dès la
  première version Android, on inscrit 12 personnes de l'équipe Marriott et le
  compteur tourne pendant qu'on avance sur le reste.

**Pas de SASU maintenant.** Ça ajoute un comptable et des frais fixes avant le
premier euro encaissé.

**On passe en SASU au premier de ces trois signaux :**
1. un **deuxième hôtel payant** ;
2. le chiffre d'affaires approche le **plafond micro** pour les services (environ
   77 k€/an — vérifier le chiffre de l'année) ;
3. le **service achats** d'un hôtel exige une société comme fournisseur.

Ce n'est pas un piège : Apple permet de migrer un compte individuel vers une
organisation (via le support), et Google permet de transférer les apps vers un
nouveau compte. Rien de ce qu'on publie maintenant n'est perdu.

### Individuel ou société : ce qu'il faut savoir

**Prendre société, pas individuel.** Deux raisons.

1. **Google bloque les comptes personnels.** Un compte individuel ouvert depuis fin
   2023 doit faire tourner un test fermé avec **12 testeurs pendant 14 jours de
   suite** avant d'avoir le droit de publier en production. Un compte société en
   est dispensé. (À revérifier sur la page — Google bouge ces règles.)
2. **Ce que voit le client.** En individuel, l'App Store affiche votre nom
   personnel comme vendeur. En société, il affiche l'entreprise. Devant un
   directeur d'hôtel, ce n'est pas cosmétique.

**Le D-U-N-S est le chemin critique** : gratuit, quelques jours à deux semaines, et
il débloque le compte société **des deux côtés**. À demander en premier, avant même
de payer.

## « C'est une web app qu'on dépose, ou une vraie app ? »

**Une vraie app.** Pas un raccourci, pas un marque-page. Capacitor produit un vrai
`.ipa` (iOS) et un vrai `.aab` (Android) : des binaires installés depuis le store,
avec une icône, présents dans le sélecteur d'apps, qui fonctionnent hors ligne.

Dedans :

- vos écrans, **embarqués comme fichiers dans l'app** — pas chargés depuis un site,
  donc ça ouvre instantanément et ça marche sans réseau ;
- un pont qui laisse ce code appeler **le vrai téléphone** : appareil photo pour le
  BEO, notifications par les systèmes d'Apple et de Google, fichiers, vibration.

L'image : une voiture. Ce qu'on a dessiné, c'est la carrosserie, le tableau de bord,
les sièges. Capacitor est le châssis — il ne redessine pas l'intérieur, il rend le
véhicule homologué.

**Ce qu'Apple contrôle** : la règle 4.2 (« minimum functionality ») rejette les apps
qui ne sont qu'un site web dans une boîte. Alba passe sans difficulté — photo,
notifications, hors ligne. À savoir pour ne pas être surpris à la première revue.

**Pour mettre à jour** : on modifie, on recompile, on redépose. Revue en 1 à 3 jours
en général ; Google est souvent le jour même après la première.
