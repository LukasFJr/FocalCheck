# Simulateur de focale (équivalent 35 mm) 🎥📐📱

Outil web (une seule page) pour **prévisualiser des focales équivalentes plein format** directement depuis le navigateur, via la **caméra arrière** du téléphone. 🎯📏🧪

* **Aucune dépendance** : un unique `index.html`
* **Caméra** via `getUserMedia` (HTTPS/localhost requis)
* **Cadre simulé** mis à l’échelle (base ÷ cible) + **FOV h×v** calculés
* **Ratios** : Auto (flux), 16:9, 4:3, 3:2, 1:1, 9:16
* **Diagnostics** intégrés : sécurité, permissions, devices, logs bornés

---

## Démo rapide ⚡️🚀🔒

1. Héberger en **HTTPS** (GitHub Pages recommandé) ou lancer en **localhost**.
2. Ouvrir la page publiée → **Activer la caméra** → **Autoriser**.
3. Régler **Focale de base** (ex. 24 mm pour la caméra principale iPhone récents ; certains modèles : 26 mm).
4. Choisir une **focale simulée** (slider/puces) et l’**aspect** (ex. 16:9).
5. Astuce : toucher l’écran avec **deux doigts** → toggle de la grille.

---

## Déploiement (GitHub Pages) 🌐📦✅

### Option A — Interface GitHub 🖥️⬆️🔧

* Déposer `index.html` à la racine du dépôt : **Code → Add file → Upload files** → **Commit**.
* **Settings → Pages** → **Build and deployment** :

  * **Source** = *Deploy from a branch*
  * **Branch** = *main* / *(root)* → **Save**
* L’URL finale : `https://<votre-user>.github.io/<votre-repo>/` (HTTPS auto).

### Option B — Ligne de commande 🧑‍💻💾📤

```bash
# Dans le dossier qui contient index.html
git init
git add index.html
git commit -m "Deploy focal simulator"
git branch -M main
git remote add origin git@github.com:<votre-user>/<votre-repo>.git  # ou https://...
git push -u origin main
# Puis Settings → Pages comme ci‑dessus
```

> **Important** : l’aperçu de fichier sur `github.com` (vue *blob*) **n’a pas** accès à la caméra. Utilisez l’URL `https://<user>.github.io/<repo>/`. 🔒📷✅

---

## Utilisation & précision 🎛️🎯🧭

* **Focale de base (équiv. 35 mm)** : focale *équivalente* du module utilisé (par défaut 24 mm). Ajuster si nécessaire.
* **Focale simulée** : la cible à prévisualiser (35, 50, 85, 135, 200…)
* **FOV (h×v)** : calculés pour le **même ratio** sélectionné (évite les biais de recadrage).
* **Alignement** : le cadre simulé suit la **zone vidéo réellement visible** (gestion des bandes noire/pilier via un conteneur overlay).

### Calibration (recommandée) 🎯📏🧪

1. Sujet fixe à distance connue.
2. Régler **50 mm** simulé.
3. Ajuster la **Focale de base** jusqu’à concordance avec une référence (photo/optique FF).
4. Les autres focales suivent ensuite avec cohérence.

---

## Résolution de problèmes 🛠️🐞🧯

* **NotAllowedError: Permission denied**

  * L’accès a été refusé : re‑autoriser via l’icône **caméra** (barre d’adresse).
  * iOS : **Réglages → Safari → Caméra → Autoriser**.
  * Vérifier **HTTPS** (ou `localhost`).
* **NotFoundError / OverconstrainedError** : aucune caméra détectée ou contraintes trop strictes → bouton **Réessayer** (l’app teste plusieurs contraintes) ou redémarrer le navigateur.
* **SecurityError** : contexte non sécurisé (HTTP, iFrame sans `allow="camera"`).
* **Écran noir** : fermer les autres apps qui utilisent la caméra ; tester un autre navigateur à jour.

---

## Détails techniques 🧠🔬⚙️

* **Caméra** : `navigator.mediaDevices.getUserMedia()` avec fallback de contraintes

  * `{ video: { facingMode: { exact: 'environment' } } }`
  * `{ video: { facingMode: 'environment' } }`
  * `{ video: true }`
* **Modèle optique** : échelle = `base_eq / target_eq` (même format d’image).
* **FOV** : cadre 35 mm 36×24 recadré au ratio choisi ; `FOV = 2 * atan(dim / (2 * f_eq))` (h et v).
* **Perf** : pas de `box-shadow` géant ; masque d’assombrissement en 4 bandes ; animations GPU (`will-change: transform`).
* **Logs** : **regroupés** avec `requestAnimationFrame` et **bornés** à 200 entrées.
* **Énergie** : arrêt caméra automatique sur `visibilitychange`/`pagehide`.

---

## Compatibilité 📱🖥️🌍

| Plateforme                               | Support                                            |
| ---------------------------------------- | -------------------------------------------------- |
| **iOS Safari** (16+)                     | ✅ Recommandé. Autoriser la caméra si demandé.      |
| **Chrome Android**                       | ✅                                                  |
| **Desktop** (Chrome/Safari/Firefox/Edge) | ✅                                                  |
| **WebView / In‑App Browser**             | ⚠️ Variable selon l’implémentation `getUserMedia`. |

> Contexte **sécurisé** requis : **HTTPS** ou `localhost`. Les pages `file://` sont bloquées. 🔐🌐✅

---

## Structure du dépôt 🗂️📄🏗️

```
index.html        # application autonome (UI + styles + logique + diagnostics)
```

---

## Crédits 🙌🎬💡

Conception & développement : moi : ) ✨🧑‍💻🎨

Si vous faites un fork, merci de conserver la mention d’origine et d’ouvrir des PR pour les améliorations majeures. 🔀🙏🚀
