# KAIROS — application installable (PWA)

Ce dossier contient une application 100% autonome (aucun serveur/backend requis) :

```
index.html          → l'application elle-même
manifest.json        → identité de l'app (nom, icônes, couleur) pour l'installation
service-worker.js    → mise en cache pour le fonctionnement hors-ligne + mises à jour auto
icons/                → icônes utilisées sur l'écran d'accueil (Android et iPhone)
```

## Important : HTTPS obligatoire

Les service workers (donc le mode hors-ligne et l'installation) ne fonctionnent
que sur une adresse en **https://** (ou sur `localhost` pour tester en local).
Un simple double-clic sur `index.html` sur ton ordinateur ne suffira pas.

## Héberger (au choix)

**Firebase Hosting** (comme tes autres projets) :
```bash
npm install -g firebase-tools
firebase login
firebase init hosting   # choisir ce dossier comme "public"
firebase deploy
```

**GitHub Pages** :
1. Pousse ce dossier dans un dépôt GitHub (branche `main`, ou un dossier `/docs`).
2. Repo → Settings → Pages → choisir la branche/dossier.
3. L'app sera disponible sur `https://<utilisateur>.github.io/<repo>/`.

Les deux fonctionnent avec ce dossier tel quel : tous les chemins sont relatifs
(`./manifest.json`, `./icons/...`), donc pas de configuration d'URL de base à faire.

## Mises à jour automatiques

Le service worker charge toujours la page en ligne en priorité quand l'utilisateur
a du réseau, et ne retombe sur la version enregistrée que s'il est hors-ligne.
Donc : tu redéploies (`firebase deploy` ou un `git push`), et au prochain lancement
avec réseau, chaque étudiant reçoit automatiquement la nouvelle version — sans
rien réinstaller.

Astuce : si un changement ne semble pas arriver chez un utilisateur, monte le
numéro `CACHE_VERSION` en haut de `service-worker.js` (ex. `kairos-v2`) — cela
force le nettoyage de l'ancien cache.

## Installer sur le téléphone

- **Android (Chrome)** : un bouton « Installer l'application » apparaît automatiquement
  dans l'app une fois hébergée en https. Sinon : menu ⋮ → « Installer l'application ».
- **iPhone (Safari)** : Safari ne propose pas d'installation automatique. Le bouton
  affiche l'instruction : appuyer sur *Partager* → *Sur l'écran d'accueil*.

## Données des étudiants

Chaque planning est stocké uniquement dans le navigateur de l'étudiant
(`localStorage`), sur son propre appareil. Rien n'est envoyé à un serveur :
pas de compte à créer, mais aussi pas de sauvegarde partagée entre appareils
pour l'instant.
