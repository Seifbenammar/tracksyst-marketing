# 🚀 DEPLOY.md — Déploiement du site marketing TrackSyst

> Procédure de déploiement du contenu de **ce repo `tracksyst-marketing`**
> vers l'hébergement OVH du domaine `tracksyst.cloud`.
>
> Ce repo est **distinct** du repo applicatif `tracksyst-app` (qui sert
> `app.tracksyst.cloud`). Séparation propre : vitrine commerciale ≠ produit SaaS.

---

## 📋 Vue d'ensemble

Le site marketing TrackSyst est servi depuis :

| URL publique | Fichier source (racine du repo) |
|---|---|
| `https://tracksyst.cloud/` | `index.html` |
| `https://tracksyst.cloud/smsi/` | `smsi/index.html` |
| `https://tracksyst.cloud/mentions-legales.html` | `mentions-legales.html` |
| `https://tracksyst.cloud/plaquette-smsi-v1.html` | `plaquette-smsi-v1.html` |
| `https://tracksyst.cloud/404.html` (auto via .htaccess) | `404.html` |
| Configuration Apache | `.htaccess` |
| Assets | `assets/branding/*.svg`, `assets/favicon/*.png` |

**Important** :
- ⚠️ Ce site marketing est **distinct** de l'application : `app.tracksyst.cloud` reste servi par `public/` du repo applicatif `tracksyst-app`.
- ⚠️ Le site marketing n'a **aucune dépendance** : HTML auto-suffisant, SVG inlinés, CSS inline, zéro CDN externe.
- ⚠️ Les assets sont **dupliqués** depuis `tracksyst-app/public/assets/` pour autonomie. En cas de mise à jour du logo, patcher les deux repos.

---

## 🎯 Structure cible serveur OVH

Sur l'hébergement OVH lié à `tracksyst.cloud` (en racine) :

```
/www/                              (racine de tracksyst.cloud)
├── index.html                     ← marketing/index.html (landing racine)
├── 404.html                       ← marketing/404.html (page d'erreur)
├── mentions-legales.html          ← marketing/mentions-legales.html
├── .htaccess                      ← marketing/.htaccess (HTTPS + sécurité)
├── favicon.ico                    ← copie de public/favicon.ico
├── smsi/
│   └── index.html                 ← marketing/smsi/index.html (landing SMSI)
├── assets/                        (optionnel)
│   ├── favicon/                   ← copies public/assets/favicon/*
│   └── branding/                  ← copies public/assets/branding/*.svg (logos référencés depuis CSS si besoin)
└── (futurs sous-modules)
    ├── smq/   ← V2
    ├── sme/   ← V2
    ├── sst/   ← V2
    └── sme-energie/  ← V2
```

---

## ⚙ Création du sous-domaine et SSL OVH

> **À faire UNE seule fois** au moment du setup initial.

1. Manager OVH → **Web Cloud → Hébergements → tracksyst.cloud**
2. Onglet **Multisite** → vérifier que `tracksyst.cloud` (racine, sans préfixe) pointe vers un dossier dédié (créer `/www/` ou `/marketing/` si pas déjà fait).
3. SSL → **Let's Encrypt** activé (renouvellement auto OVH).
4. Vérification : `https://tracksyst.cloud/` doit déjà afficher quelque chose (page par défaut OVH avant deploy).

⚠️ **Ne pas confondre avec `app.tracksyst.cloud`** : c'est un autre sous-domaine OVH qui pointe vers le repo applicatif TrackSyst SMSI (`/www/app/` ou équivalent OVH).

---

## 📤 Workflow d'upload — Option A : FileZilla (recommandé V1.1)

> Le plus simple pour les premiers déploiements et corrections rapides.

### Première fois

1. Ouvrir **FileZilla** → connexion FTP/SFTP OVH (credentials fournis par OVH Manager → Hébergements → tracksyst.cloud → FTP-SSH).
2. Naviguer dans la racine `/www/` du domaine `tracksyst.cloud`.
3. **Glisser-déposer** depuis l'explorateur Windows `C:\Users\seifb\OneDrive\10-DEV\03-TrackSyst\marketing\` :
   - `index.html`
   - `404.html`
   - `mentions-legales.html`
   - `.htaccess` (⚠️ FileZilla : cocher "Forcer afficher les fichiers cachés" — menu Serveur → Forcer affichage)
   - dossier `smsi/` entier
4. Copier aussi `favicon.ico` depuis `public/favicon.ico` à la racine de `/www/` du marketing.
5. (Optionnel) Copier le dossier `public/assets/favicon/` vers `/www/assets/favicon/` pour que les meta favicons fonctionnent.
6. **Vérification** :
   - `https://tracksyst.cloud/` → landing racine
   - `https://tracksyst.cloud/smsi/` → landing SMSI
   - `https://tracksyst.cloud/mentions-legales.html` → mentions légales
   - `https://tracksyst.cloud/cette-url-nexiste-pas` → 404.html personnalisée
   - `https://tracksyst.cloud/smsi` (sans trailing slash) → redirige vers `/smsi/` (.htaccess fait le job)

### Mises à jour ultérieures

À chaque modification d'un fichier dans `marketing/` :
1. Upload **uniquement** le(s) fichier(s) modifié(s).
2. Cache navigateur : tester en mode incognito ou Ctrl+F5.

---

## 📤 Workflow d'upload — Option B : Git Deploy OVH (V2)

> Pour automatiser les déploiements (recommandé quand le rythme augmente).

### Mise en place initiale

1. Créer un repo GitHub privé **distinct** : `tracksyst-marketing` (ou utiliser un sous-dossier du repo principal avec config OVH adaptée).
2. Y pousser le contenu de `marketing/` (renommer en racine du repo).
3. Manager OVH → Hébergement → Multisite → `tracksyst.cloud` → **Déploiement Git** → activer, choisir branche `main`.
4. Générer la clé SSH OVH, l'ajouter en Deploy Key sur le repo GitHub.

### Workflow quotidien

```bash
# Depuis le repo tracksyst-marketing local
cd C:\Projets\tracksyst-marketing
git pull origin main
# ... éditer les fichiers ...
git add .
git commit -m "feat: ajout module XYZ"
git push origin main
# Auto-deploy OVH déclenché ~30 secondes après
```

⚠️ Si on choisit Option B, **maintenir un repo dédié** plutôt que coupler avec le repo applicatif `tracksyst-app` (risque de fuite d'informations légales/commerciales dans un repo de code).

---

## 🔒 Sécurité du déploiement

| Vérification | Comment |
|---|---|
| HTTPS forcé | `http://tracksyst.cloud` doit rediriger en 301 vers `https://...` |
| Headers sécurité | Inspecter `Response Headers` avec DevTools : présence de `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, `Content-Security-Policy` |
| Pas de listing | `https://tracksyst.cloud/smsi/` doit afficher la page, jamais le contenu du dossier |
| Pas d'accès aux configs | `https://tracksyst.cloud/DEPLOY-MARKETING.md` → 403 (bloqué par .htaccess) |
| 404 personnalisée | Tester une URL random → vérifier que `/404.html` s'affiche bien |

---

## 🧪 Tests post-déploiement

Checklist à dérouler après chaque deploy majeur :

- [ ] Landing racine charge en < 1 seconde (Lighthouse Performance > 90)
- [ ] Lighthouse SEO > 90 (méta complètes, schema.org, OG tags)
- [ ] Lighthouse Accessibility > 90 (contrastes, aria-labels)
- [ ] Mobile responsive (iPhone SE 375px + tablet 768px)
- [ ] Tous les `mailto:` ouvrent le client mail avec le bon sujet pré-rempli
- [ ] Tous les liens internes (`/smsi/`, `#modules`, `#contact`, etc.) fonctionnent
- [ ] Lien externe `https://app.tracksyst.cloud` ouvre l'app
- [ ] `mentions-legales.html` lisible et à jour
- [ ] Footer affiche bien **WebManiTech SARL** + RC 74599 + ICE
- [ ] Tests partage social : LinkedIn / Twitter aperçu OG OK
- [ ] HSTS : activable dans .htaccess après ~1 mois de stabilité HTTPS

---

## 📞 Support

Pour toute question sur la procédure : `contact@webmanitech.com`.

En cas de modification du branding (nouvelle adresse siège, nouvelle ICE, etc.), patcher **les 3 endroits** :
1. `marketing/index.html` footer
2. `marketing/smsi/index.html` footer
3. `marketing/mentions-legales.html` corps de page

Puis re-déployer.
