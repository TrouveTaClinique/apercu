# Aperçu brouillon — Trouve ta clinique

Dépôt d’**aperçu uniquement** pour l’organisation [TrouveTaClinique](https://github.com/TrouveTaClinique).

| | |
|---|---|
| **Rôle** | Prévisualiser la branche `brouillon` du site avant publication |
| **URL prévue** | `https://apercu.trouvetaclinique.ca` (après DNS + Pages) |
| **Ne sert pas** | `trouvetaclinique.ca` ni la production |

Le contenu est déployé automatiquement depuis
[`TrouveTaClinique.github.io`](https://github.com/TrouveTaClinique/TrouveTaClinique.github.io)
(branche `brouillon`), après passage dans `scripts/preparer-apercu.js`
(noindex, titre BROUILLON, sans CNAME de production).

## Réglages Pages (à faire une fois)

1. Settings → Pages → Source : **GitHub Actions**
2. Custom domain : `apercu.trouvetaclinique.ca` (quand le DNS sera prêt)
3. Ne jamais y mettre `trouvetaclinique.ca`

## DNS EasyDNS (plus tard)

```text
CNAME  apercu  →  trouvetaclinique.github.io
```

(ou la cible indiquée par GitHub dans Settings → Pages)
