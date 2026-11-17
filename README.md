# 3D-Gaussian-Splatting-Reconstruction

## Présentation

Ce projet consiste à reconstruire une scène 3D à partir d'une courte séquence vidéo avec la méthode **3D Gaussian Splatting**.

Une vidéo filmée autour d'un bureau est utilisée pour extraire des images, estimer les poses des caméras, entraîner un modèle de gaussiennes 3D et évaluer la qualité des rendus. Le projet explore le comportement de la méthode sur une séquence vidéo à très faible variation de point de vue.

## Pipeline

1. **Extraction des images :** 56 images extraites de la vidéo à 5 images/s avec FFmpeg.
2. **Estimation des poses de caméra :** COLMAP estime les paramètres des caméras et produit une reconstruction éparse.
3. **Préparation des données :** correction de la distorsion des images (taille d'origine conservée) et préparation pour l'entraînement.
4. **Entraînement :** 3D Gaussian Splatting (implémentation de référence de Graphdeco-INRIA), 7 000 itérations. Une image sur huit est mise de côté pour le test (49 images d'entraînement, 7 de test).
5. **Rendu :** rendu des vues d'entraînement et de test à partir du modèle.
6. **Évaluation :** PSNR, SSIM et LPIPS sur les 7 vues de test.

## Résultats

| Paramètre / Métrique | Résultat |
|---|---:|
| Nombre d'images | 56 (49 entraînement, 7 test) |
| Itérations d'entraînement | 7 000 |
| Nombre de gaussiennes | 93 693 |
| SSIM | 0.9667 |
| PSNR | 34.95 dB |
| LPIPS | 0.1636 |


### Visualisations

**Captures des résultats :**
![Vérité terrain vs rendu 3DGS](r%C3%A9sultats/comparaison.png)

**Vidéo de reconstruction :** 

https://github.com/user-attachments/assets/59d03371-715d-4d84-8040-cae9d4b22d61

## Difficultés rencontrées

- **Recadrage excessif de COLMAP :** avec les paramètres par défaut, la dé-distorsion produisait des images de 1309 × 144 au lieu de 1280 × 720 (limite `min_scale = 0,2`). Corrigé avec `--min_scale 1 --max_scale 1`.

## Technologies utilisées

- Python
- PyTorch (via l'implémentation de référence)
- COLMAP
- 3D Gaussian Splatting
- FFmpeg
- Google Colab (GPU T4)

## Reproduire

Le notebook `3D_gaussian_splatting.ipynb` regroupe toutes les étapes (installation, extraction des images, COLMAP, entraînement, rendu, métriques). Il s'exécute sur Google Colab avec un GPU T4.


