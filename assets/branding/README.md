# Branding TrackSyst SMSI — logos combinés

Fichiers vectoriels prêts à consommer pour les usages futurs où l'on a
besoin du logo TrackSyst SMSI complet (icône + nom + tagline) plutôt
que du seul favicon (icône seule).

## Variantes disponibles

| Fichier | Format | Ratio | Cas d'usage |
|---|---|---|---|
| `tracksyst-logo.svg` | 400×80 (bandeau horizontal) | 5:1 | En-tête de rapport PDF, signature email, bandeau supérieur d'une présentation |
| `tracksyst-logo-stacked.svg` | 200×200 (carré vertical) | 1:1 | En-tête A4 portrait, splash screen, première de couverture, écran d'accueil |

## Composition

Les deux variantes partagent :

- **Icône** : `bi-shield-check` Bootstrap Icons v1.11.3 (path identique au favicon, commit `26f9ff1`)
- **Couleur primaire** : `#0d6efd` (Bootstrap primary, identique au reste de l'app)
- **Couleur texte secondaire** : `#495057` (gris neutre)
- **Police** : sans-serif système — `'Helvetica Neue', Helvetica, Arial, sans-serif`
- **Fond** : transparent (pas de rectangle blanc — la transparence permet l'intégration sur tout fond clair)
- **Texte vectoriel** : sélectionnable, indexable, scaling propre à toutes les tailles

## Cohérence visuelle

Ces logos sont alignés sur l'identité visuelle déjà déployée :

- Favicon (`public/assets/favicon/`) — même icône bouclier sur fond #0d6efd
- Brand-header partial (`src/Views/partials/brand-header.php`) — même iconographie
- Navbar — `<i class="bi bi-shield-check"></i>` à côté du nom

## Usages prévus (Phase 6 et au-delà)

### Rapports PDF d'audit ISO 27001 (Phase 6)

```html
<!-- En-tête de rapport horizontal -->
<img src="/assets/branding/tracksyst-logo.svg" alt="TrackSyst SMSI" height="60">

<!-- Page de couverture verticale -->
<img src="/assets/branding/tracksyst-logo-stacked.svg" alt="TrackSyst SMSI" width="180">
```

### Avec dompdf (si choisi en Phase 6)

dompdf supporte `<img src="...svg">` avec [SVG library](https://github.com/dompdf/dompdf/wiki/Image-Support). Référencer
les fichiers via `Options::setIsRemoteEnabled(true)` ou via chemin local
absolu.

### Avec mPDF

mPDF supporte les SVG natifs ; en cas de souci de rendu sur certaines
versions, convertir préalablement en PNG via le script
`scripts/generate-favicon.py` (cairosvg) ou ImageMagick côté CI.

### Signature email automatique (futur)

Le `tracksyst-logo.svg` peut être inline-styled dans un template HTML
email. Attention : Outlook 2016+ supporte SVG inline mais Outlook
Word-renderer (versions plus anciennes) **ne le rend pas**. Pour les
emails larges audience, prévoir une déclinaison PNG + fallback alt
text. Pour les emails internes / techniques, le SVG suffit.

## Modifications futures

Ces fichiers sont **commités tels quels** et statiques. Pas de script
de génération automatique, pas de lien dynamique.

Si besoin :

- Variante claire pour fond sombre → créer `tracksyst-logo-light.svg`
  (icône blanche, texte blanc) plutôt que de modifier les existants.
- Variante monochrome (impression noir et blanc) → créer
  `tracksyst-logo-mono.svg`.
- Versions raster (PNG) si une lib PDF gère mal le SVG → utiliser
  `cairosvg` pour conversion ad hoc, NE PAS commiter de PNG ici (ce
  dossier est réservé aux sources vectorielles).

## Aucune intégration UI dans cette version

Aucun écran existant n'utilise actuellement ces fichiers. Ils sont
prêts à consommer le jour où Phase 6 (rapports PDF) ou un autre besoin
les requiert.

Pour l'identité visuelle dans l'app web actuelle, voir :

- `src/Views/partials/brand-header.php` (header brandé pour pages auth)
- `src/Views/partials/navbar.php` (logo navbar pour pages admin)
- `public/assets/favicon/` (favicon pack PNG/ICO)
