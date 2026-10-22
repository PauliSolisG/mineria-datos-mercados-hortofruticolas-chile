# Minería de Datos: precios y volúmenes de frutas y hortalizas en Chile

Proyecto de minería de datos desarrollado con la metodología **CRISP-DM** sobre
precios y volúmenes de frutas y hortalizas en mercados mayoristas de Chile.
Incluye análisis exploratorio, preparación de datos, modelos supervisados
(regresión y clasificación) y modelos no supervisados (clustering).

> Proyecto académico de la asignatura Minería de Datos 2, Ingeniería en Infomática, DuocUC.


## Objetivos

- Analizar el comportamiento de precios y volúmenes.
- **Regresión:** predecir el volumen por unidad (`Volumen_Unidad`).
- **Clasificación:** predecir el mercado de comercialización entre los 4 más frecuentes.
- **Clustering:** segmentar los productos para descubrir patrones.

## Datos

- **Fuente:** [Chilean Fruit and Veg. Wholesale Price & Volume](https://www.kaggle.com/datasets/sebastiantarebustos/odepa-chilean-fruit-and-vegetables-volume-price) (Kaggle), basado en datos de ODEPA.
- **Contenido:** precios y volúmenes diarios por mercado, producto y unidad.
- ~49.000 registros entre 2021 y 2022.
- Variables principales: Producto, Variedad, Mercado, Unidad, Fecha, Precio, Volumen, Latitud y Longitud.
- Variables creadas en este proyecto: `Precio_tratado`, `Precio_Unidad` [ajusta si creaste otras].
- Para ejecutar el notebook, descarga el archivo desde Kaggle y súbelo cuando la primera celda lo pida.

## Metodología (CRISP-DM)

1. **Comprensión del negocio:** objetivos, KPIs (precio promedio, volumen total por mercado, variación de precios).
2. **Comprensión de los datos:** estadística descriptiva, valores nulos, distribución de precios, outliers y correlaciones.
3. **Preparación de los datos:**
   - Imputación de "Sin especificar" en `Variedad` con la moda por producto (afectaba al 20,32 % de los registros).
   - Tratamiento de outliers con el método IQR por producto.
   - Creación de `Precio_Unidad` y `Precio_tratado`.
   - Escalado de variables para K-Means.
4. **Modelado:** cuatro ejecuciones por modelo, con distintas combinaciones de variables.
5. **Evaluación:** R² y RMSE (regresión), Accuracy y F1 (clasificación), Silhouette (clustering).
6. **Validación:** validación cruzada de 5 particiones y validación final con datos de diciembre de 2022.

División temporal para la clasificación: entrenamiento (ene 2021 – may 2022), prueba (jun – nov 2022) y validación (dic 2022).

## Resultados

### Regresión: predecir `Volumen_Unidad`

| Modelo | Mejor combinación de variables | R² test | RMSE test |
|---|---|---|---|
| **XGBoost** | Precio_Unidad + Volumen | **0.9571** | **3175.59** |
| KNN | Precio_Unidad + Volumen | 0.9565 | 3826.02 |
| Árbol de Decisión | Precio_Unidad + Volumen | 0.9286 | 4899.09 |
| Random Forest | Precio_tratado + Volumen | 0.9283 | 4910.95 |
| Regresión Lineal | Precio_tratado + Volumen | [valor] | [valor] |

Validación cruzada (5 folds) de XGBoost: R² promedio [valor], desviación [valor].

### Clasificación: predecir el mercado (top 4)

| Modelo | Variables | Accuracy test | F1 test |
|---|---|---|---|
| **Random Forest** | Precio, Numero_Unidad, Producto | **0.6631** | **0.6651** |
| XGBoost | Todas las variables | 0.6390 | 0.6417 |
| KNN | Precio, Numero_Unidad, Producto, Variedad | 0.6293 | 0.6302 |
| Árbol de Decisión | Precio, Numero_Unidad, Producto, Variedad | 0.4808 | [valor] |

Validación final (diciembre 2022) con Random Forest: **Accuracy 0.5857, F1 0.5906**.

### Clustering (k = 4, método del codo)

| Modelo | Silhouette |
|---|---|
| K-Means | 0.8974 |
| Clustering Jerárquico (Ward) | 0.8924 |

K-Means sobre precio y volumen separó cuatro perfiles: productos de precio medio y bajo volumen (el más frecuente), productos de precio alto y muy bajo volumen, productos de precio medio-bajo y mayor volumen (los de más demanda), y un grupo intermedio.

## Conclusiones

- Las variables de precio combinadas con el volumen fueron las que mejor predijeron `Volumen_Unidad` en todos los modelos.
- XGBoost fue el mejor modelo de regresión y Random Forest el mejor de clasificación.
- El rendimiento en la validación de diciembre 2022 (Accuracy 0.5857) fue menor que en test (0.6631), lo que muestra que el modelo pierde precisión con datos nuevos.
- El clustering permitió identificar segmentos de productos útiles como apoyo a decisiones comerciales.

## Limitaciones

- `Volumen` se usa como variable predictora de `Volumen_Unidad`; como están relacionadas, el R² puede estar sobreestimado.
- Los outliers se reemplazaron por la media del producto, lo que reduce la variabilidad real de los precios.
- La clasificación del mercado tiene un desempeño moderado y no se afinaron hiperparámetros.
- El período cubierto es corto (2021–2022), por lo que no se capturan patrones estacionales de largo plazo.

## Tecnologías

Python · pandas · NumPy · scikit-learn · XGBoost · matplotlib · seaborn · Google Colab

## Cómo ejecutarlo

1. Abre el notebook en Google Colab:
   [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO/NOMBRE_DEL_REPO/blob/main/mineria_datos_2.ipynb)
2. Descarga el dataset desde Kaggle y súbelo cuando la primera celda lo solicite.
3. Ejecuta todas las celdas en orden (*Entorno de ejecución → Ejecutar todo*).

## Estructura del repositorio

```
├── mineria_datos_2.ipynb 
├── README.md
└── [datos/archivo.xlsx]   
```
## Autores

| Nombre | Contacto | 
|---|---|
| **Paulina Solís** | pauli.solis00@gmail.com |
| **Diego Román** | diego.alonsoromanp@gmail.com | 
