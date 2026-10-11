# ¿Se puede anticipar el resultado de un partido de selecciones?

Proyecto final · **Python para Ciencia de Datos** · 2026

**Integrantes:** Juan Apolo · Alen Cukier

---

## De qué se trata

Intentamos predecir el resultado de un partido de selecciones (victoria local, empate o victoria visitante) usando únicamente información disponible **antes** de que empiece el partido: la fuerza histórica de cada equipo, su forma reciente y el contexto del encuentro.

El resultado corto: se puede, hasta cierto punto. Un random forest acierta el **61%** de los partidos contra el **48%** de predecir siempre "gana el local". La variable que más aporta es la diferencia de puntaje Elo entre los dos equipos. El modelo, en cambio, prácticamente no logra predecir empates, y en el informe explicamos por qué.

## Los datos

[International football results from 1872 to 2026](https://www.kaggle.com/datasets/martj42/international-football-results-from-1872-to-2017), de Mart Jürisoo (Kaggle, licencia CC0). Son 49.144 partidos de selecciones masculinas mayores hasta el 31/12/2025.

El archivo está incluido en `datos/results.csv`, así que el proyecto corre sin descargar nada.

## Qué hay en el repositorio

```
├── analisis_futbol.qmd      # documento principal: código + texto
├── analisis_futbol.ipynb    # el mismo documento como notebook, para Positron
├── informe_futbol.html      # informe final en HTML
├── informe_futbol.pdf       # informe final en PDF
├── datos/
│   └── results.csv          # datos originales
├── requirements.txt
└── README.md
```

## Cómo reproducirlo

```bash
pip install -r requirements.txt
quarto render analisis_futbol.qmd
```

También se puede abrir `analisis_futbol.ipynb` en Positron, VS Code o Jupyter y correr las celdas una por una.

## Qué hicimos

1. **Limpieza.** La base viene muy limpia (sin faltantes ni duplicados). Cortamos en el 31/12/2025 porque 2026 está incompleto.
2. **Variables construidas.** Calculamos un puntaje **Elo** propio (K = 20, ventaja de 60 puntos para el local en cancha propia) y la **forma reciente** de cada selección en sus últimos 5 partidos. Las dos se calculan recorriendo los partidos en orden cronológico, leyendo el estado antes de cada partido y actualizándolo después, para que nunca se use información del futuro.
3. **Análisis exploratorio.** Partidos por año, distribución de resultados según la sede, goles por partido, relación entre la diferencia de Elo y el resultado, correlaciones, y la trayectoria del Elo de Uruguay.
4. **Modelado.** Partición cronológica (entrenamiento hasta 2018, evaluación de 2019 a 2025) y comparación de tres modelos: una regla de referencia, un árbol de decisión y un random forest con hiperparámetros elegidos por `GridSearchCV`.

## Herramientas

`numpy` · `pandas` · `matplotlib` · `seaborn` · `scikit-learn`
