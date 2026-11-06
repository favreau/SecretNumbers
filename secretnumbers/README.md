# Secret Numbers

Jeu web immersif : explore **4 mondes fractals** (parmi 16 : Mandelbrot, Julia, Burning Ship, Tricorn…), trouve le **chiffre doré** camouflé dans chaque panorama 360°, puis ouvre le **cadenas**.

Les fractales sont rendues en direct via un shader WebGL (Three.js). Fichier unique `index.html` — GitHub Pages + mobile.

## Jouer en local

Ouvre `index.html` via un serveur HTTP (les modules ES / Three.js CDN le demandent) :

```bash
python3 -m http.server 8080
```

Puis va sur [http://localhost:8080](http://localhost:8080).

## GitHub Pages

1. Pousse ce dépôt sur GitHub.
2. Settings → Pages → Source : branche `main`, dossier `/ (root)`.
3. Le jeu sera servi à `https://<user>.github.io/<repo>/`.

## Dépendances

- [Three.js](https://threejs.org/) chargé depuis unpkg (réseau requis au premier chargement).
