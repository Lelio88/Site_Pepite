# Site Pépite — Contexte d'Opération et Garde-Fous Agentiques

Résolvez les problèmes sans introduire de régression ni de dette technique architecturale.

## I. Finalité

**Application** : Site Pépite — pages web publiques de Pépite (accueil, confidentialité, conditions, mentions légales).
**Objectif métier** : fournir les adresses que les plateformes exigent pour une application (écran de consentement Google, TikTok, Meta) : accueil, politique de confidentialité, conditions d'utilisation. Publié sur `https://pepite.heianenterprise.com`. Le code de Pépite, lui, reste dans son dépôt privé.

## II. Architecture

**Modèle** : site statique pur (HTML/CSS, aucun build, aucun JavaScript), servi par GitHub Pages depuis la racine de `main`. **Détails** : ce fichier suffit.

- `index.html` — accueil ; `confidentialite.html`, `suppression-donnees.html` (exigée par Meta : comment supprimer ses données et retirer l'accès), `cgu.html`, `mentions-legales.html` — pages légales
- `legal.css` — feuille commune (thème sombre orange) ; `icone.png` — icône de Pépite (copie de `Pepite/packaging/pepite.png`)
- `CNAME` — `pepite.heianenterprise.com` (enregistrement DNS CNAME vers `lelio88.github.io`, « DNS only », chez Cloudflare)

## III. Pile Technologique

HTML5 et CSS3 vanilla. Aucune dépendance, aucun gestionnaire de paquets.

## IV. Garde-Fous non négociables

1. **Zéro ressource tierce, zéro JavaScript, aucun style en ligne** : tout vient de `legal.css`.
2. **La politique de confidentialité dit vrai** : toute fonction de Pépite qui contacte un service ou traite une donnée personnelle (nouvelle plateforme, nouvel envoi) la met à jour **dans le même commit** et change sa date. Elle est lue par Google, TikTok et Meta lors de leurs audits.
3. **Ne jamais y décrire une fonction qui n'existe pas encore comme disponible** : TikTok et Instagram y sont dits « à terme ».
4. **Chaque page porte `<meta name="tdm-reservation" content="1" />`** (refus de fouille par les robots d'entraînement IA) et le lien « Mentions légales » dans sa navigation.
5. **Contact public unique** : `heianenterpriseyt@gmail.com`.

## V. Flux de Travail (Explore → Plan → Code → Verify)

1. Lire les pages voisines pour calquer le ton et la structure
2. Modifier, puis ouvrir la page dans un navigateur à 320 px et 1440 px
3. Vérifier les liens (aucun mort) et que `grep -c "<script\|style=" *.html` rend 0

## VI. Commandes de Développement

```bash
start index.html   # aucune commande de build : ouvrir la page
```

## VII. Maintenance documentaire

**Règle d'or** : le diff du contenu et celui de la doc correspondante vont dans **le même commit**.

| Modification | Fichier à mettre à jour |
|---|---|
| Pépite contacte un nouveau service, ou traite une nouvelle donnée | `confidentialite.html` (+ sa date) |
| Nouvelle plateforme de publication branchée | `confidentialite.html`, `cgu.html`, `index.html` |
| Sous-domaine ou hébergeur qui change | `CNAME` + `mentions-legales.html` + `INFRASTRUCTURE.md` du conteneur |

## VIII. Contexte de Session

- **Dernier focus** : création du site pour l'écran de consentement Google (accès YouTube de Pépite).
- **Focus immédiat** : DNS `pepite` chez Cloudflare, puis remplir les champs « Branding » de la console Google Cloud.
