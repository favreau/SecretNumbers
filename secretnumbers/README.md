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

Déployé automatiquement à chaque push sur `main` :

`https://favreau.github.io/SecretNumbers/secretnumbers/`

## Dépendances

- [Three.js](https://threejs.org/) chargé depuis unpkg (réseau requis au premier chargement).
