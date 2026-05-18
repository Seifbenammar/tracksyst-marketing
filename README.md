# tracksyst-marketing

> Repo dédié aux pages marketing publiques de **TrackSyst**, servies sur
> [https://tracksyst.cloud](https://tracksyst.cloud).
>
> Repo distinct du repo applicatif [`tracksyst-app`](https://github.com/Seifbenammar/tracksyst-app)
> pour autonomie de déploiement et séparation propre vitrine ≠ produit.

---

## 📂 Structure du repo

```
/
├── index.html              ← Landing racine tracksyst.cloud (5 modules QSEÉ)
├── 404.html                ← Page d'erreur cohérente charte
├── mentions-legales.html   ← Éditeur WebManiTech SARL + hébergeur OVH
├── plaquette-smsi-v1.html  ← Plaquette commerciale PDF (4 pages A4)
├── smsi/
│   └── index.html          ← Landing module SMSI tracksyst.cloud/smsi/
├── assets/
│   ├── branding/           ← Logos TrackSyst SVG (autonomes)
│   ├── favicon/            ← Pack favicons multi-tailles
│   └── css/                ← (Vide pour l'instant — CSS inline dans les HTML)
├── favicon.ico             ← Favicon racine (servi à /favicon.ico)
├── .htaccess               ← Apache : HTTPS forcé, sécurité, 404 perso
├── DEPLOY.md               ← Procédure de déploiement OVH
└── README.md               ← Ce fichier
```

## 🚀 Routes publiques (tracksyst.cloud)

| URL | Fichier |
|---|---|
| `https://tracksyst.cloud/` | `index.html` (landing racine 5 modules) |
| `https://tracksyst.cloud/smsi/` | `smsi/index.html` (landing SMSI) |
| `https://tracksyst.cloud/mentions-legales.html` | mentions légales |
| `https://tracksyst.cloud/plaquette-smsi-v1.html` | plaquette PDF |
| `https://tracksyst.cloud/404.html` | page d'erreur (servie automatiquement via `.htaccess`) |

## 🎯 Caractéristiques techniques

- **HTML self-contained** : aucune dépendance CDN externe (CSS inline, SVG inlinés)
- **Mobile-first responsive** (breakpoints 640px / 768px / 1024px)
- **SEO complet** : meta title/description/keywords + Open Graph + Twitter Card + schema.org Organization
- **Accessibilité** : HTML5 sémantique, aria-labels, contrastes WCAG AA
- **Performance** : ~50 Ko par page (gzippable ~10 Ko)
- **Cookieless** : zéro tracking, zéro analytics tiers, zéro cookie (RGPD-by-design)
- **Sécurité** : `.htaccess` impose HTTPS, headers CSP/X-Frame/X-Content-Type, blocage `.md`/`.env`

## 🏢 Éditeur

**WebManiTech SARL** — RC Kénitra 74599 — IF 66050358 — ICE 003564783000079
Siège : Lot 403, Immeuble 10, Étage 3, Mehdia 2, Kénitra — Maroc
SARL au capital de 100 000 MAD — [contact@webmanitech.com](mailto:contact@webmanitech.com)

**Hébergement utilisateurs** : OVH SAS, France (données stockées en UE — conformité RGPD).

## 📤 Déploiement

Voir [`DEPLOY.md`](DEPLOY.md) pour la procédure complète (FileZilla ou Git deploy OVH).

## 🔗 Liens

- **Application SaaS** : [https://app.tracksyst.cloud](https://app.tracksyst.cloud) (repo `tracksyst-app`)
- **Repo applicatif** : [github.com/Seifbenammar/tracksyst-app](https://github.com/Seifbenammar/tracksyst-app)
- **Contact commercial** : [contact@webmanitech.com](mailto:contact@webmanitech.com)
- **Support produit** : [support@tracksyst.cloud](mailto:support@tracksyst.cloud)

---

© 2026 TrackSyst — édité par WebManiTech SARL · Tous droits réservés.
