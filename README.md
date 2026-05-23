# Análisis GLM

Análisis estadístico de dos bases de datos mediante Modelos Lineales Generalizados (GLM), implementado en R con Quarto. El informe cubre desde la exploración de datos hasta la selección y diagnóstico de modelos.

## Descripción

El trabajo aborda dos contextos de modelado distintos:

**Base 1 — `fishing.csv`**  
Datos ecológicos de 147 lances de muestreo marino. La variable de respuesta (`totabund`) corresponde al conteo total de peces capturados. El análisis evalúa y compara tres familias de distribución para datos de conteo sobredispersos:

- Poisson (modelo base y diagnóstico de sobredispersión)
- Quasi-Poisson (estimación del parámetro de dispersión φ)
- Binomial Negativa (modelo final recomendado)

**Base 2 — `Weekly` (ISLR)**  
Datos semanales del mercado bursátil de EE.UU. (1990–2010). Se ajusta un modelo de **regresión logística** para clasificar la dirección del mercado (`Up`/`Down`) a partir de retornos rezagados y volumen de transacciones.

## Estructura del repositorio

```
Analisis-GLM/
├── Trabajo_GLM.qmd           # Documento fuente en Quarto
├── Trabajo_GLM.html          # Informe renderizado
├── fishing.csv               # Dataset 1: abundancia de peces
├── Instrucciones..pdf        # Enunciado del trabajo
├── css/
│   └── styles.css            # Estilos personalizados del informe HTML
└── Trabajo_GLM_files/
    └── figure-commonmark/    # Figuras generadas durante el render
```

## Variables principales

### fishing.csv

| Variable     | Tipo        | Descripción                                      |
|--------------|-------------|--------------------------------------------------|
| `totabund`   | Conteo (Y)  | Abundancia total de peces por lance              |
| `meandepth`  | Continua    | Profundidad media del área de muestreo (metros)  |
| `sweptarea`  | Continua    | Área barrida por la red (esfuerzo de muestreo)   |
| `density`    | Continua    | Densidad de follaje/hábitat en el fondo marino   |
| `period`     | Categórica  | Período de muestreo: `1977-1989` o `2000-2002`   |

### Weekly (ISLR)

Datos de retornos semanales del S&P 500 con rezagos `Lag1`–`Lag5`, `Volume`, `Today` y la variable respuesta `Direction`.

## Metodología

```
EDA univariado y bivariado
  └─ Detección de outliers (IQR + Distancia de Mahalanobis)
  └─ Análisis de multicolinealidad (VIF)

Modelado GLM — Fishing
  ├─ GLM Poisson             → diagnóstico de sobredispersión (φ ≈ 41)
  ├─ GLM Quasi-Poisson       → corrección de errores estándar
  └─ GLM Binomial Negativa   → modelo final con AIC y devianza

Modelado GLM — Weekly
  └─ Regresión Logística     → curva ROC, AUC, matriz de confusión
```

## Tecnologías

- **R** (≥ 4.2)
- **Quarto** (≥ 1.3) para el documento reproducible
- Paquetes principales: `tidyverse`, `MASS`, `pROC`, `car`, `GGally`, `kableExtra`, `patchwork`, `moments`, `broom`, `ISLR`

## Reproducir el informe

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/MValenzuelaN/Analisis-GLM.git
   cd Analisis-GLM
   ```

2. Instalar las dependencias de R (se usa `pacman` para la gestión automática):

   ```r
   install.packages("pacman")
   pacman::p_load(tidyverse, broom, kableExtra, ggrepel, knitr, car,
                  GGally, moments, pROC, MASS, patchwork, ISLR)
   ```

3. Renderizar el documento:

   ```bash
   quarto render Trabajo_GLM.qmd
   ```

   El informe `Trabajo_GLM.html` se generará en el directorio raíz.

> [!NOTE]
> El archivo `fishing.csv` debe estar en el directorio raíz junto al `.qmd` para que la carga de datos funcione correctamente.

## Resultados clave

- El modelo Poisson presentó sobredispersión severa (φ ≈ 41), invalidando sus inferencias.
- El modelo **Binomial Negativa** se identificó como el más adecuado para `totabund`, capturando la agregación natural de los peces.
- `density` resultó el predictor más relevante (r = 0.947 con `totabund`); `meandepth` mostró un efecto negativo moderado.
- Para el dataset `Weekly`, la regresión logística permitió evaluar la capacidad predictiva de los retornos rezagados sobre la dirección del mercado.

## Autor

**Matías Valenzuela Nuche**
