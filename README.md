# Aperçu brouillon — Trouve ta clinique

Dépôt d’**aperçu uniquement** pour l’organisation [TrouveTaClinique](https://github.com/TrouveTaClinique).

| | |
|---|---|
| **Rôle** | Prévisualiser la branche `brouillon` du site avant publication |
| **URL prévue** | `https://apercu.trouvetaclinique.ca` |
| **URL GitHub (en attendant le DNS)** | `https://trouvetaclinique.github.io/apercu/` |
| **Ne sert pas** | `trouvetaclinique.ca` ni la production Map |

Le contenu est synchronisé automatiquement depuis
[`TrouveTaClinique.github.io`](https://github.com/TrouveTaClinique/TrouveTaClinique.github.io)
(branche `brouillon`), après `scripts/preparer-apercu.js`
(noindex, titre BROUILLON, CNAME `apercu.trouvetaclinique.ca` uniquement).

## Réglages Pages (une fois)

1. Ouvrir [Settings → Pages](https://github.com/TrouveTaClinique/apercu/settings/pages)
2. **Source** : GitHub Actions
3. **Custom domain** : `apercu.trouvetaclinique.ca` (après l’enregistrement DNS, ou dès que le fichier `CNAME` du site le demande)
4. Cocher **Enforce HTTPS** quand GitHub le propose
5. **Ne jamais** y mettre `trouvetaclinique.ca` ni `www.trouvetaclinique.ca`

Via API (après `gh auth login`), équivalent :

```bash
gh api -X POST repos/TrouveTaClinique/apercu/pages \
  -f build_type=workflow \
  -f source[branch]=main \
  -f source[path]=/
```

Puis, quand le DNS répond :

```bash
gh api -X PUT repos/TrouveTaClinique/apercu/pages \
  -f cname=apercu.trouvetaclinique.ca \
  -F build_type=workflow
```

## Checklist DNS EasyDNS

Zone : `trouvetaclinique.ca` (EasyDNS). **Ajouter** un enregistrement ; ne pas modifier l’apex ni `www` de production.

| Étape | Action | Valeur |
|---|---|---|
| 1 | Type | `CNAME` |
| 2 | Hôte / Name | `apercu` |
| 3 | Cible / Points to | `trouvetaclinique.github.io.` (avec le point final si EasyDNS le demande) |
| 4 | TTL | 300 à 3600 s |
| 5 | Attendre la propagation | `nslookup apercu.trouvetaclinique.ca` |
| 6 | Vérifier Pages | Settings → Pages : domaine vérifié, HTTPS OK |
| 7 | Contrôle contenu | Ouvrir `https://apercu.trouvetaclinique.ca/` → titre `BROUILLON \| …`, `noindex` |

**Ne pas faire**

- Transférer le domaine Map → TTC
- Changer Pages du dépôt principal
- Merger `brouillon` → `main`
- Pointer `apercu` vers un autre hébergeur que GitHub Pages

## Secret requis sur le dépôt principal

Sur `TrouveTaClinique/TrouveTaClinique.github.io` → Settings → Secrets → Actions :

- `APERCU_DEPLOY_TOKEN` : jeton fine-grained (ou classic) avec accès en écriture au dépôt `TrouveTaClinique/apercu` (`contents: write`, et lecture Pages si possible)

Le workflow `Aperçu du brouillon` pousse ensuite le site préparé dans ce dépôt ; le workflow `pages.yml` ci-dessous publie via Actions.
