# mdm-parent-apk

Dépôt dédié à la génération de l'APK Android de l'espace parent de
Maître de Maison, via GitHub Actions + Bubblewrap (Trusted Web
Activity). Ce dépôt ne contient pas le code de l'application elle-même
(serveur, HTML) — il empaquette simplement le site déjà en ligne sur
Render (https://maitre-de-maison-1.onrender.com) sous forme d'app
Android installable.

## Mise en route

1. **Secrets GitHub** — Settings → Secrets and variables → Actions,
   ajouter :
   - `ANDROID_KEYSTORE_BASE64`
   - `ANDROID_KEYSTORE_PASSWORD`
   - `ANDROID_KEY_PASSWORD`

   (valeurs communiquées séparément — ne jamais les committer en clair)

2. **assetlinks.json** — le fichier `assetlinks.json` à la racine de ce
   dépôt n'est PAS utilisé par GitHub Actions : il doit être copié dans
   le dépôt du **serveur** (maitre-de-maison), dans
   `public/.well-known/assetlinks.json`, pour qu'il soit servi à
   `https://maitre-de-maison-1.onrender.com/.well-known/assetlinks.json`.
   Sans ça, l'APK s'ouvre avec la barre d'adresse Chrome visible
   (fonctionnel, mais moins "app native").

3. **Lancer le build** — onglet "Actions" de ce dépôt → "Build APK —
   Espace Parent" → "Run workflow". L'APK signé est ensuite
   téléchargeable dans les "Artifacts" de l'exécution.

## Fichiers

- `android-twa/twa-manifest.json` — configuration Bubblewrap (nom,
  couleurs, icônes, URL du manifest PWA à empaqueter)
- `android-twa/android.keystore` — clé de signature de l'app. À
  conserver précieusement : la perdre empêche de publier une mise à
  jour sous le même package Android.
- `.github/workflows/build-apk-parent.yml` — le workflow qui construit
  l'APK à la demande.
- `assetlinks.json` — voir point 2 ci-dessus.

## Mettre à jour l'app après un changement du site

Rien à faire ici : comme il s'agit d'une Trusted Web Activity, l'app
Android affiche toujours le contenu live de
maitre-de-maison-1.onrender.com. Il n'est nécessaire de relancer un
build que pour changer le nom, l'icône, les couleurs ou la version de
l'app elle-même (modifier `twa-manifest.json` puis relancer le
workflow).
