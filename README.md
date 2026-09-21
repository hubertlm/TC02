# TC02 — Face Recognition with Classical Pattern Classifiers

Second computational assignment for **TI0097 – Introduction to Pattern Recognition** (Prof. Guilherme de Alencar Barreto, Department of Teleinformatics Engineering, Federal University of Ceará – UFC).

The goal is to build and compare classical statistical classifiers on a face-recognition problem, and to study how normalization and dimensionality reduction (PCA, Box-Cox) change their behavior.

> The assignment statement and the written report are in Portuguese (see [`PROJETO2_FACE_2025.pdf`](PROJETO2_FACE_2025.pdf) and [`TC02_Relatório-3.pdf`](TC02_Relatório-3.pdf)).

## Dataset

The **Yale Face Database A**: 15 subjects × 11 conditions (center/left/right light, glasses / no glasses, happy, sad, sleepy, surprised, wink, normal), stored in [`Faces/`](Faces). Each image is resized (e.g. 20×20), vectorized by stacking its columns, and saved together with its subject label to a `recfaces.dat` file (one sample per row: features + label).

## Classifiers

| Classifier | Idea |
|---|---|
| Gaussian quadratic classifier | One covariance matrix per class |
| Variant 1 | Tikhonov-regularized covariance |
| Variant 2 | Single pooled covariance matrix |
| Variant 3 | Friedman regularization |
| Variant 4 | Naive-Bayes-like (diagonal covariance) |
| Least-squares linear classifier | Linear discriminant fitted by least squares |
| 1-NN | Nearest training sample |
| DMC | Minimum distance to the class centroid |
| MaxCorr | Maximum correlation with the class centroid |

## Experiments

Every experiment repeats a random 80 % / 20 % train/test split **50 times** and reports the mean, standard deviation, minimum, maximum and median accuracy, plus execution time and the number of singular covariance matrices:

1. Raw vectorized images, without PCA.
2. PCA with no dimensionality reduction (decorrelated features).
3. PCA keeping 98 % of the variance.
4. Box-Cox transformation followed by PCA (98 % variance).
5. **Access-control (intruder detection)** with 11 extra images of the author acting as the "intruder", comparing (a) Mahalanobis distance on the vectorized images and (b) PCA + z-score + Euclidean distance, reporting accuracy, false-positive / false-negative rates, sensitivity and precision.

## Repository structure

```
TC02/
├── Codigos_Octave/          # Instructor-provided Octave scripts
│   ├── face_preprocessing_column.m   # load, resize and vectorize the faces (optional PCA)
│   ├── compara_todos.m               # compare the quadratic classifiers and variants
│   ├── myDMC.m  myKNN.m  Maxcorr.m   # distance/correlation-based classifiers
├── Faces/                   # Yale A images
├── Resolucao/               # Our solution
│   ├── TC02.ipynb                    # Python notebook: all classifiers, PCA, Box-Cox, intruder detection
│   └── recfaces_*.dat                # generated datasets (raw, PCA variants, Box-Cox, with intruder)
├── PROJETO2_FACE_2025.pdf   # Assignment statement (PT)
└── TC02_Relatório-3.pdf     # Written report (PT)
```

## Running it

- **Python solution:** open `Resolucao/TC02.ipynb` in Jupyter (needs `numpy`, `scipy`, `pandas`, `scikit-learn`, `matplotlib` and `opencv-python`) and run all cells. Dataset paths are relative to the notebook, so start Jupyter from `Resolucao/`.
- **Octave scripts:** requires [GNU Octave](https://octave.org/) with the `image` package (`pkg load image`). Run `face_preprocessing_column.m` from the folder that contains the face images to produce `recfaces.dat`, then `compara_todos.m`.

## Author

Hubert Luz de Miranda — Computer Engineering, UFC.
