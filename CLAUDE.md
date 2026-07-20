# Log de sondage — Diagnostic pollution des sols

## Objectif
Application web mono-page pour générer des **logs (coupes) de sondage orientés diagnostic de pollution des sols (SSP)**. L'utilisateur saisit une fiche de sondage et ses couches géologiques ; l'appli dessine le log à l'échelle avec les **figurés géologiques normalisés**, et permet l'impression / export PDF au format A4.

## Contexte métier
- Domaine : sites et sols pollués (SSP), diagnostic de pollution.
- Au-delà de la lithologie, un log doit faire apparaître les **indices de pollution** : mesures **PID** (ppm), indices **organoleptiques** (odeurs hydrocarbures / H₂S / solvants, colorations, taches), les **échantillons** prélevés (sol / eau / gaz du sol) et le **niveau d'eau**.
- Figurés : nomenclature géologique classique (type BRGM) — argile = tirets horizontaux, sable / grès = pointillés, grave / galets = ronds, calcaire / marne = briques, argilite = traits obliques, etc. La légende doit rester conforme à cette convention.

## État actuel
- Un seul fichier : `index.html` (HTML + CSS + JavaScript **vanilla**, sans framework ni étape de build).
- Le log est rendu en **SVG** ; les figurés sont de vraies formes vectorielles répétées manuellement
  (fonction `tiledFig`, avec découpe `clip-path`) — **pas** de `<pattern>` SVG. Chromium rasterise les
  remplissages `<pattern>` en basse résolution à l'impression / export PDF ; le tuilage manuel garde un
  rendu 100 % vectoriel, identique à l'écran et à l'impression.
- Fonctions présentes : fiche de sondage, couches (de / à, lithologie, description, PID, indice organoleptique), échantillons, niveau d'eau, échelle réglable (px/m), impression A4, téléchargement PDF direct.
- Interface pensée pour un **usage de terrain** (tablette, téléphone) autant que pour un poste de bureau : mise en page à une colonne, champs à 16px (évite le zoom automatique de Safari iOS) et cibles tactiles agrandies sous 880px de large.

## Contraintes techniques (impératives)
- Hébergement **GitHub Pages** → **tout doit rester côté client** : pas de backend, pas d'appel serveur, pas de build.
- Chemins **relatifs** uniquement (`./…`), jamais absolus (`/…`).
- **Aucun secret ni clé API** dans le code (le dépôt est public).
- Pas de `localStorage` / `sessionStorage`, pas d'export / import JSON : aucune persistance de session,
  le log se transmet via le PDF (impression ou téléchargement direct).
- Rester **léger et sans dépendances lourdes** ; le code doit rester lisible pour un non-développeur.
- Interface et libellés **en français**. Les données chiffrées (profondeurs, valeurs) sont en police monospace pour l'alignement.

## Conventions de travail
- Un commit = un changement compréhensible ; **ouvrir une pull request** pour toute évolution.
- L'appli doit rester **fonctionnelle à tout moment** (le fichier s'ouvre par simple double-clic dans un navigateur).
- Ne **jamais casser** l'impression A4, le téléchargement PDF, ni l'affichage sur mobile / tablette.
- Toujours tester le bouton « Exemple » après une modification : il doit continuer à produire un log complet.

## Pistes / objectifs ouverts
- Colonne optionnelle **« équipement / piézomètre »** : crépine, massif filtrant, bentonite, cimentation, tube plein.
- Pouvoir **caler les figurés** sur une légende de bureau ou une norme précise, si un modèle est fourni.
- **Clarifier « LNE »** (laboratoire ? norme ? référentiel interne ?) et l'intégrer si pertinent.
- Enrichir la liste des **lithologies** au besoin (argile sableuse, tourbe, remblais différenciés, etc.).
