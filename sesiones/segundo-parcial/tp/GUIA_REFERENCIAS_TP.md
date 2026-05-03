# 📚 Guía de Referencias para el Trabajo Práctico Integrador
## Métodos Predictivos para la Gestión - UBA TGAD

**Dataset:** `colorado_real_estate_2026.csv`  
**Fecha:** Mayo 2026  
**Equipo:** [Nombres de los integrantes]

---

## 📋 ÍNDICE

1. [Selección de Datos y Preprocesamiento](#i-selección-de-datos-y-preprocesamiento)
2. [Muestreo e Inferencia](#ii-muestreo-e-inferencia)
3. [Modelado Estadístico Clásico](#iii-modelado-estadístico-clásico)
4. [Machine Learning y Validación](#iv-machine-learning-y-validación)
5. [Preguntas Obligatorias - Referencias](#preguntas-obligatorias)

---

## i) Selección de Datos y Preprocesamiento (Unidad 2)

### 1.1 Limpieza de Datos

#### **Valores Faltantes, Outliers y Duplicados**

**📁 Archivo Principal:** [metodos-tgad-fce/sesiones/primer-parcial/u1-estrategias-de-muestreo/ejercicios/Titanic.ipynb](../../../metodos-tgad-fce/sesiones/primer-parcial/u1-estrategias-de-muestreo/ejercicios/Titanic.ipynb)

**Contenido:**
- **Listwise deletion:** Eliminar filas con cualquier valor NA
- **Pairwise deletion:** Usar pares válidos en análisis
- **Imputación por moda:** Completar valores faltantes
- **Ubicación:** Celdas 1-6 del notebook

**Código clave:**
```python
# Listwise deletion
listwise_df = data.dropna()

# Identificar valores faltantes
print(data.isna().sum())

# Imputación por moda
for col in num_cols:
    mode_value = mode_df[col].mode(dropna=True)[0]
    mode_df[col].fillna(mode_value, inplace=True)
```

---

**📁 Archivo Complementario:** [taller-tgad-fce/sesiones/primer-parcial/u2/clase10/TPAD_Práctica_U2_1ra_parte.ipynb](../../../taller-tgad-fce/sesiones/primer-parcial/u2/clase10/TPAD_Práctica_U2_1ra_parte.ipynb)

**Ejercicio 1 - Detección de valores extremos:**
- **Celda:** `#VSC-3bc72b46`
- **Función:** Detectar y tratar valores faltantes
- **Función:** Detectar duplicados con `df.duplicated()`

**Código clave:**
```python
# Detectar valores faltantes
df.isna().sum()

# Eliminar duplicados
df.drop_duplicates()

# Cantidad de casos por categoría
df['columna'].value_counts()
```

---

### 1.2 Detección de Outliers (Boxplots y Z-score)

**📁 Archivo Principal:** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/estadistica_descriptiva.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/estadistica_descriptiva.ipynb)

**Función detectar_outliers - Regla de Tukey:**
- **Líneas:** 258-274
- **Método:** Outlier si `x < Q1 - 1.5*RIQ` o `x > Q3 + 1.5*RIQ`

**Código clave:**
```python
def detectar_outliers(datos):
    """Regla de Tukey: outlier si x < Q1 - 1.5*RIQ  o  x > Q3 + 1.5*RIQ"""
    q1 = np.percentile(datos, 25)
    q3 = np.percentile(datos, 75)
    riq = q3 - q1
    
    lim_inf = q1 - 1.5 * riq
    lim_sup = q3 + 1.5 * riq
    
    outliers = [x for x in datos if x < lim_inf or x > lim_sup]
    
    return outliers, lim_inf, lim_sup

# Ejemplo de uso
outliers, li, ls = detectar_outliers(df['columna'])
print(f"Outliers detectados: {outliers}")
```

**Ejemplo aplicado:**
- **Línea:** 684+
- **Ejercicio 1:** Análisis de tiempos de respuesta de API con detección de outliers

---

### 1.3 Análisis Descriptivo

#### **Medidas de Posición, Dispersión y Forma**

**📁 Archivo Principal:** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/estadistica_descriptiva.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/estadistica_descriptiva.ipynb)

**Sección 1.1 - Implementación manual:**
- **Media aritmética:** Línea 58+
- **Mediana:** Línea 65+
- **Moda:** Línea 77+
- **Varianza y Desvío:** Líneas siguientes
- **Percentiles y Cuartiles**

**Código clave:**
```python
# Media
def media(datos):
    return sum(datos) / len(datos)

# Mediana
def mediana(datos):
    ordenados = sorted(datos)
    n = len(ordenados)
    mitad = n // 2
    
    if n % 2 == 1:
        return ordenados[mitad]
    else:
        return (ordenados[mitad - 1] + ordenados[mitad]) / 2

# Con Pandas
df['columna'].mean()
df['columna'].median()
df['columna'].std()
df['columna'].describe()
```

---

**📁 Archivo Complementario (Caso Real):** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/estadistica_ecommerce.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/estadistica_ecommerce.ipynb)

**Análisis completo de variables:**
- **Línea 45+:** Análisis de precio unitario (media, mediana, percentiles, asimetría)
- **Línea 158+:** Comparación con/sin outliers
- **Línea 1927+:** Correlación entre variables

**Código clave:**
```python
print(f"  Media              : ${precios.mean():.4f}")
print(f"  Mediana            : ${precios.median():.4f}")
print(f"  P25 / P75          : ${precios.quantile(0.25):.2f} / ${precios.quantile(0.75):.2f}")
print(f"  Desvío estándar    : ${precios.std():.4f}")
print(f"  CV                 : {precios.std()/precios.mean()*100:.1f}%")
print(f"  Asimetría G1       : {precios.skew():.4f}")
```

---

### 1.4 Matriz de Correlación

**📁 Archivo Principal:** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/TPAD_Clase_11.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase11/teorica/estadisticas/TPAD_Clase_11.ipynb)

**Sección "Covarianza y Correlación lineal":**
- **Línea:** 2484+
- **Interpretación de correlación:** Línea 2505+

**Código clave:**
```python
# Matriz de correlación de Pearson
df.corr()

# Correlación de Spearman (más robusta ante outliers)
df.corr(method='spearman')

# Visualizar con heatmap
import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize=(10,8))
sns.heatmap(df.corr(), annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Matriz de correlación")
plt.show()
```

**Referencia en estadistica_ecommerce.ipynb:**
- **Línea 1150+:** Matriz de correlación Pearson y Spearman
- **Interpretación:** Correlación débil/moderada/fuerte

---

### 1.5 Visualizaciones (Histogramas, Boxplots, Distribución)

**📁 Archivo Principal:** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase13/teorica/TPAD_Clase_13_2_matplotlib_seaborn.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase13/teorica/TPAD_Clase_13_2_matplotlib_seaborn.ipynb)

**Contenido:**
- **Line plots:** Evolución temporal
- **Bar plots:** Comparación entre categorías
- **Histogramas:** Distribución de variables
- **Boxplots:** Detección visual de outliers
- **Comparación:** Matplotlib vs Seaborn

**Código clave:**
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Histograma con Matplotlib
plt.figure(figsize=(10,6))
plt.hist(df['columna'], bins=30, edgecolor='black')
plt.title('Distribución de [Variable]')
plt.xlabel('Valor')
plt.ylabel('Frecuencia')
plt.show()

# Histograma con Seaborn
plt.figure(figsize=(10,6))
sns.histplot(data=df, x='columna', kde=True, bins=30)
plt.title('Distribución de [Variable]')
plt.show()

# Boxplot para outliers
plt.figure(figsize=(10,6))
sns.boxplot(data=df, x='categoria', y='valor')
plt.title('Boxplot por Categoría')
plt.show()

# Pairplot para múltiples variables
sns.pairplot(df[['var1', 'var2', 'var3', 'target']])
plt.show()
```

---

## ii) Muestreo e Inferencia (Unidad 1)

### 2.1 Extracción de Muestra Aleatoria (n=750)

**📁 Archivo Principal:** [taller-tgad-fce/sesiones/primer-parcial/u2/clase10/TPAD_Práctica_U2_1ra_parte.ipynb](../../../taller-tgad-fce/sesiones/primer-parcial/u2/clase10/TPAD_Práctica_U2_1ra_parte.ipynb)

**Ejercicio:**
- **Celda:** `#VSC-b3ab6fd4`
- **Enunciado:** "Seleccione dos muestras aleatorias: una con reemplazo y otra sin reemplazo de tamaño 1000"

**Código clave:**
```python
# Muestra aleatoria simple SIN reemplazo (n=750)
muestra = df.sample(n=750, random_state=42)

# Muestra aleatoria CON reemplazo
muestra_reemplazo = df.sample(n=750, replace=True, random_state=42)

# Verificar tamaño
print(f"Tamaño de la muestra: {len(muestra)}")
print(f"Tamaño del dataset completo: {len(df)}")
```

---

### 2.2 Validación de Representatividad

**📁 Archivo Referencia:** [metodos-tgad-fce/sesiones/primer-parcial/u1-estrategias-de-muestreo/ejercicios/Titanic.ipynb](../../../metodos-tgad-fce/sesiones/primer-parcial/u1-estrategias-de-muestreo/ejercicios/Titanic.ipynb)

**Metodología:**
- Comparar media y varianza de la variable objetivo entre muestra y población
- Comparar distribuciones con estadísticos descriptivos

**Código clave:**
```python
# Comparación de estadísticos
print("Comparación Población vs Muestra")
print("="*50)
print(f"{'Estadístico':<20} {'Población':>15} {'Muestra':>15}")
print("-"*50)

# Media
media_pob = df['target'].mean()
media_muestra = muestra['target'].mean()
print(f"{'Media':<20} {media_pob:>15.4f} {media_muestra:>15.4f}")

# Varianza
var_pob = df['target'].var()
var_muestra = muestra['target'].var()
print(f"{'Varianza':<20} {var_pob:>15.4f} {var_muestra:>15.4f}")

# Desvío estándar
std_pob = df['target'].std()
std_muestra = muestra['target'].std()
print(f"{'Desvío Estándar':<20} {std_pob:>15.4f} {std_muestra:>15.4f}")

# Visualización comparativa
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

axes[0].hist(df['target'], bins=30, alpha=0.7, label='Población')
axes[0].set_title('Distribución Población')
axes[0].legend()

axes[1].hist(muestra['target'], bins=30, alpha=0.7, label='Muestra', color='orange')
axes[1].set_title('Distribución Muestra (n=750)')
axes[1].legend()

plt.tight_layout()
plt.show()
```

---

## iii) Modelado Estadístico Clásico (Unidades 3, 4 y 5)

### 3.1 Regresión Lineal

**📁 Archivo Principal:** [metodos-tgad-fce/practicas/Codigo Regresion.ipynb](../../../metodos-tgad-fce/practicas/Codigo Regresion.ipynb)

**Contenido completo:**
- Modelo de regresión lineal con **statsmodels**
- Interpretación de coeficientes
- Significatividad estadística (p-valores)
- R² y ajuste del modelo
- Gráficos de diagnóstico

**Código clave:**
```python
import pandas as pd
import statsmodels.api as sm
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Preparar datos
X = df[["var1", "var2", "var3"]]  # Variables independientes
y = df["target"]                   # Variable dependiente

# 2. Agregar constante (intercepto)
X = sm.add_constant(X)

# 3. Ajustar modelo
model = sm.OLS(y, X).fit()

# 4. Mostrar resumen completo
print(model.summary())

# 5. Gráficos de diagnóstico
residuals = model.resid
fitted = model.fittedvalues

# Gráfico de residuos vs valores ajustados
plt.figure(figsize=(10,6))
plt.scatter(fitted, residuals)
plt.axhline(0, color='red', linestyle='--')
plt.xlabel("Valores ajustados")
plt.ylabel("Residuos")
plt.title("Residuos vs Valores Ajustados")
plt.show()

# Pairplot de variables
sns.pairplot(df[["var1", "var2", "var3", "target"]])
plt.show()
```

**Interpretación del modelo:**
```python
# Extraer coeficientes
print("Coeficientes del modelo:")
print(model.params)

# Extraer R²
print(f"\nR² del modelo: {model.rsquared:.4f}")
print(f"R² ajustado: {model.rsquared_adj:.4f}")

# P-valores (significatividad)
print("\nP-valores:")
print(model.pvalues)
```

---

**📁 Archivo Complementario:** [laboratorio-fcen/clases/clase-16- RLS/practica16/metricas_regresion.py](../../../laboratorio-fcen/clases/clase-16- RLS/practica16/metricas_regresion.py)

**Métricas de evaluación:**
- **MAE** (Mean Absolute Error)
- **MSE** (Mean Squared Error)
- **RMSE** (Root Mean Squared Error)
- **R²** (Coeficiente de determinación)

**Código clave:**
```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

# Predicciones
y_pred = model.predict(X)

# Calcular métricas
mae = mean_absolute_error(y, y_pred)
mse = mean_squared_error(y, y_pred)
rmse = np.sqrt(mse)
r2 = r2_score(y, y_pred)

print(f"MAE:  {mae:.4f}")
print(f"MSE:  {mse:.4f}")
print(f"RMSE: {rmse:.4f}")
print(f"R²:   {r2:.4f}")
```

---

### 3.2 Regresión Logística (si la variable es categórica)

**Referencia:** Si tu variable objetivo es categórica (clasificación), usar **Logistic Regression**

**Código clave:**
```python
import statsmodels.api as sm

# Preparar datos (similar a regresión lineal)
X = df[["var1", "var2", "var3"]]
y = df["target_binario"]  # Variable categórica (0/1)

X = sm.add_constant(X)

# Ajustar modelo logístico
logit_model = sm.Logit(y, X).fit()

# Resumen
print(logit_model.summary())

# Métricas de clasificación
from sklearn.metrics import classification_report, confusion_matrix

y_pred_proba = logit_model.predict(X)
y_pred = (y_pred_proba > 0.5).astype(int)

print("\nMatriz de Confusión:")
print(confusion_matrix(y, y_pred))

print("\nReporte de Clasificación:")
print(classification_report(y, y_pred))
```

---

### 3.3 Comparación: Muestra (n=750) vs Base Total

**📁 Archivo Referencia:** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase12/TPAD_Práctica_U2_2da_parte.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase12/TPAD_Práctica_U2_2da_parte.ipynb)

**Metodología:**
1. Ajustar modelo con **muestra de 750 registros**
2. Ajustar modelo con **base de datos completa**
3. Comparar:
   - Coeficientes estimados (β)
   - Errores estándar
   - P-valores (significatividad)
   - R² / Accuracy

**Código clave:**
```python
# 1. Modelo con muestra (n=750)
X_muestra = muestra[["var1", "var2", "var3"]]
y_muestra = muestra["target"]
X_muestra = sm.add_constant(X_muestra)

model_muestra = sm.OLS(y_muestra, X_muestra).fit()

# 2. Modelo con base total
X_total = df[["var1", "var2", "var3"]]
y_total = df["target"]
X_total = sm.add_constant(X_total)

model_total = sm.OLS(y_total, X_total).fit()

# 3. Comparar resultados
print("COMPARACIÓN: MUESTRA vs BASE TOTAL")
print("="*70)
print(f"{'Variable':<15} {'Coef Muestra':>15} {'Coef Total':>15} {'Diferencia':>15}")
print("-"*70)

for var in X_muestra.columns:
    coef_muestra = model_muestra.params[var]
    coef_total = model_total.params[var]
    diferencia = coef_total - coef_muestra
    print(f"{var:<15} {coef_muestra:>15.4f} {coef_total:>15.4f} {diferencia:>15.4f}")

print("\n")
print(f"{'R² Muestra:':<20} {model_muestra.rsquared:.4f}")
print(f"{'R² Total:':<20} {model_total.rsquared:.4f}")

# Comparar p-valores
print("\nComparación de Significatividad (p-valores):")
print("-"*70)
for var in X_muestra.columns:
    p_muestra = model_muestra.pvalues[var]
    p_total = model_total.pvalues[var]
    sig_muestra = "***" if p_muestra < 0.01 else ("**" if p_muestra < 0.05 else ("*" if p_muestra < 0.1 else ""))
    sig_total = "***" if p_total < 0.01 else ("**" if p_total < 0.05 else ("*" if p_total < 0.1 else ""))
    print(f"{var:<15} {p_muestra:>10.4f} {sig_muestra:<4} | {p_total:>10.4f} {sig_total:<4}")
```

---

## iv) Machine Learning y Validación (Unidad 6)

### 4.1 División Train/Test

**📁 Archivo Principal:** [laboratorio-fcen/clases/Clase-18-SeleccionModelos/ejemplo_seleccion_modelos.py](../../../laboratorio-fcen/clases/Clase-18-SeleccionModelos/ejemplo_seleccion_modelos.py)

**Código clave:**
```python
from sklearn.model_selection import train_test_split

# Separar features (X) y target (y)
X = df.drop('target', axis=1)
y = df['target']

# Split 80% train, 20% test
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print(f"Train: {len(X_train)} registros")
print(f"Test:  {len(X_test)} registros")
```

---

### 4.2 Algoritmos de ML

**Referencias:**
- **Regresión:** [laboratorio-fcen/clases/clase-16- RLS/practica16/](../../../laboratorio-fcen/clases/clase-16- RLS/practica16/)
- **Clasificación:** [laboratorio-fcen/clases/Clase-14-15-Clasificacion/practica14-15/](../../../laboratorio-fcen/clases/Clase-14-15-Clasificacion/practica14-15/)

**Código clave (ejemplo: Random Forest para regresión):**
```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_squared_error, r2_score

# 1. Decision Tree
dt = DecisionTreeRegressor(random_state=42, max_depth=5)
dt.fit(X_train, y_train)
y_pred_dt = dt.predict(X_test)

# 2. Random Forest
rf = RandomForestRegressor(n_estimators=100, random_state=42, max_depth=10)
rf.fit(X_train, y_train)
y_pred_rf = rf.predict(X_test)

# 3. Evaluar
print("Decision Tree:")
print(f"  RMSE: {np.sqrt(mean_squared_error(y_test, y_pred_dt)):.4f}")
print(f"  R²:   {r2_score(y_test, y_pred_dt):.4f}")

print("\nRandom Forest:")
print(f"  RMSE: {np.sqrt(mean_squared_error(y_test, y_pred_rf)):.4f}")
print(f"  R²:   {r2_score(y_test, y_pred_rf):.4f}")
```

**Código clave (ejemplo: clasificación):**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import classification_report, confusion_matrix, roc_auc_score

# 1. Decision Tree
dt_clf = DecisionTreeClassifier(random_state=42, max_depth=5)
dt_clf.fit(X_train, y_train)
y_pred_dt = dt_clf.predict(X_test)

# 2. Random Forest
rf_clf = RandomForestClassifier(n_estimators=100, random_state=42, max_depth=10)
rf_clf.fit(X_train, y_train)
y_pred_rf = rf_clf.predict(X_test)

# 3. Evaluar
print("Decision Tree - Matriz de Confusión:")
print(confusion_matrix(y_test, y_pred_dt))
print("\nReporte de Clasificación:")
print(classification_report(y_test, y_pred_dt))

print("\n" + "="*50)
print("Random Forest - Matriz de Confusión:")
print(confusion_matrix(y_test, y_pred_rf))
print("\nReporte de Clasificación:")
print(classification_report(y_test, y_pred_rf))

# ROC-AUC
y_pred_proba_rf = rf_clf.predict_proba(X_test)[:, 1]
print(f"\nROC-AUC: {roc_auc_score(y_test, y_pred_proba_rf):.4f}")
```

---

### 4.3 Métricas de Evaluación

#### **Para Regresión:**
- **MAE** (Mean Absolute Error)
- **MSE** (Mean Squared Error)
- **RMSE** (Root Mean Squared Error)
- **R²** (Coeficiente de Determinación)

#### **Para Clasificación:**
- **Matriz de Confusión**
- **Precisión** (Precision)
- **Recuperación/Recall** (Sensitivity)
- **F1-Score**
- **Curva ROC-AUC**

**Código completo:**
```python
from sklearn.metrics import (
    mean_absolute_error, mean_squared_error, r2_score,
    confusion_matrix, classification_report, 
    precision_score, recall_score, f1_score, roc_curve, auc
)
import matplotlib.pyplot as plt

# REGRESIÓN
if problema == "regresion":
    mae = mean_absolute_error(y_test, y_pred)
    mse = mean_squared_error(y_test, y_pred)
    rmse = np.sqrt(mse)
    r2 = r2_score(y_test, y_pred)
    
    print(f"MAE:  {mae:.4f}")
    print(f"MSE:  {mse:.4f}")
    print(f"RMSE: {rmse:.4f}")
    print(f"R²:   {r2:.4f}")

# CLASIFICACIÓN
else:
    # Matriz de confusión
    cm = confusion_matrix(y_test, y_pred)
    print("Matriz de Confusión:")
    print(cm)
    
    # Métricas
    precision = precision_score(y_test, y_pred, average='binary')
    recall = recall_score(y_test, y_pred, average='binary')
    f1 = f1_score(y_test, y_pred, average='binary')
    
    print(f"\nPrecisión:    {precision:.4f}")
    print(f"Recall:       {recall:.4f}")
    print(f"F1-Score:     {f1:.4f}")
    
    # Reporte completo
    print("\nReporte de Clasificación:")
    print(classification_report(y_test, y_pred))
    
    # Curva ROC
    y_pred_proba = modelo.predict_proba(X_test)[:, 1]
    fpr, tpr, thresholds = roc_curve(y_test, y_pred_proba)
    roc_auc = auc(fpr, tpr)
    
    plt.figure(figsize=(8,6))
    plt.plot(fpr, tpr, color='darkorange', lw=2, label=f'ROC (AUC = {roc_auc:.2f})')
    plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('Curva ROC')
    plt.legend(loc="lower right")
    plt.show()
```

---

### 4.4 Control de Overfitting

**Técnicas:**
1. **Validación cruzada (Cross-validation)**
2. **Ajuste de hiperparámetros**
3. **Regularización**

**Código clave:**
```python
from sklearn.model_selection import cross_val_score, GridSearchCV

# 1. Validación cruzada
scores = cross_val_score(rf, X_train, y_train, cv=5, scoring='r2')
print(f"R² promedio (CV): {scores.mean():.4f} (+/- {scores.std():.4f})")

# 2. GridSearch para hiperparámetros
param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [5, 10, 15, None],
    'min_samples_split': [2, 5, 10]
}

grid_search = GridSearchCV(
    RandomForestRegressor(random_state=42),
    param_grid,
    cv=5,
    scoring='r2',
    n_jobs=-1
)

grid_search.fit(X_train, y_train)

print(f"Mejores parámetros: {grid_search.best_params_}")
print(f"Mejor R² (CV): {grid_search.best_score_:.4f}")

# Modelo final con mejores parámetros
best_model = grid_search.best_estimator_
```

---

## 📝 Preguntas Obligatorias - Referencias Específicas

### **1. Exploración de la Identidad y el Contexto**

**📁 Archivo:** [taller-tgad-fce/sesiones/primer-parcial/u2/clase9/teorica/TPAD_Clase_9_pandas_manipulacion_datos.ipynb](../../../taller-tgad-fce/sesiones/primer-parcial/u2/clase9/teorica/TPAD_Clase_9_pandas_manipulacion_datos.ipynb)

**Preguntas a responder:**

#### ¿Cuál es la fuente de los datos?
```python
# Describir el dataset
print("Fuente: Colorado Real Estate 2026")
print("Tipo de observación: Cada fila representa una propiedad inmobiliaria")
```

#### ¿Qué representa cada fila?
```python
# Primeras filas
df.head()
df.tail()
df.sample(10)
```

#### Dimensiones del dataset
```python
# Cantidad de filas y columnas
print(f"Filas: {df.shape[0]}")
print(f"Columnas: {df.shape[1]}")
```

**Celda referencia:** `#VSC-49f6b2ae` (df.shape)

---

### **2. Inventario y Tipología de Variables**

**📁 Mismo archivo anterior**

#### Clasificar variables
```python
# Tipos de datos
df.dtypes

# Información completa
df.info()

# Nombres de columnas
columnas = df.columns.tolist()
print(columnas)
```

**Celda referencia:** `#VSC-0487ea19` (df.info)

#### Identificar variable target
```python
# Para regresión: variable numérica continua (ej: precio, listPrice, sqft)
# Para clasificación: variable categórica (ej: type, sub_type)

# Verificar tipo
print(df['listPrice'].dtype)  # float64 → regresión
print(df['type'].dtype)        # object → clasificación
```

#### Rangos de variables numéricas
```python
# Resumen estadístico
df.describe()

# Rango específico
print(f"Rango de listPrice: {df['listPrice'].min()} - {df['listPrice'].max()}")

# Verificar escalas muy diferentes
print("\nRangos de todas las variables numéricas:")
for col in df.select_dtypes(include=[np.number]).columns:
    print(f"{col}: {df[col].min():.2f} - {df[col].max():.2f}")
```

---

### **3. Identificación de "Ruido" y Variables No Informativas**

**📁 Archivo:** [taller-tgad-fce/sesiones/primer-parcial/u2/clase10/TPAD_Práctica_U2_1ra_parte.ipynb](../../../taller-tgad-fce/sesiones/primer-parcial/u2/clase10/TPAD_Práctica_U2_1ra_parte.ipynb)

#### Variables de identificación única
```python
# Buscar variables con valores únicos por fila
for col in df.columns:
    if df[col].nunique() == len(df):
        print(f"{col} tiene valores únicos por fila (probablemente ID)")
```

#### Variables con varianza cero
```python
# Buscar columnas constantes
for col in df.columns:
    if df[col].nunique() == 1:
        print(f"{col} tiene varianza cero (todos los valores son iguales)")
```

#### Variables redundantes
```python
# Matriz de correlación alta (>0.9)
corr_matrix = df.corr()
high_corr = np.where(np.abs(corr_matrix) > 0.9)
high_corr = [(corr_matrix.index[x], corr_matrix.columns[y], corr_matrix.iloc[x, y]) 
             for x, y in zip(*high_corr) if x != y and x < y]

print("Variables altamente correlacionadas (posible redundancia):")
for var1, var2, corr in high_corr:
    print(f"  {var1} <-> {var2}: {corr:.4f}")
```

#### Porcentaje de datos faltantes
```python
# Por columna
missing_pct = (df.isna().sum() / len(df) * 100).sort_values(ascending=False)
print("Porcentaje de valores faltantes por columna:")
print(missing_pct[missing_pct > 0])

# Visualizar
import matplotlib.pyplot as plt

plt.figure(figsize=(10,6))
missing_pct[missing_pct > 0].plot(kind='bar')
plt.title('Porcentaje de Valores Faltantes por Columna')
plt.ylabel('Porcentaje (%)')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```

---

### **4. Definición del Problema y Encuadre del Enfoque**

#### Pregunta de negocio/investigación
```markdown
**Ejemplo para dataset de bienes raíces:**

"¿Podemos predecir el precio de venta de una propiedad (listPrice) 
basándonos en sus características (metros cuadrados, habitaciones, 
baños, ubicación, etc.)?"

**Enfoque:** Regresión (variable continua)

**Hipótesis inicial:**
- Los metros cuadrados (sqft) serán el predictor más importante
- La cantidad de habitaciones y baños influyen positivamente
- El tipo de propiedad (single_family vs condo) afecta el precio
```

#### Valor de negocio
```markdown
**¿Quién usaría esta información?**
- Agencias inmobiliarias para tasar propiedades
- Compradores para identificar propiedades subvaloradas
- Inversores para estimar retorno de inversión

**Decisiones basadas en el modelo:**
- Establecer precios competitivos
- Identificar oportunidades de inversión
- Evaluar tendencias del mercado
```

---

## 🎯 Ejercicios Prácticos Resueltos

### **Ejercicio: Análisis Descriptivo Completo**

**📁 Referencia:** [taller-tgad-fce/sesiones/segundo-parcial/u2/clase12/TPAD_Práctica_U2_2da_parte.ipynb](../../../taller-tgad-fce/sesiones/segundo-parcial/u2/clase12/TPAD_Práctica_U2_2da_parte.ipynb)

**Celda:** `#VSC-80702879`

**Preguntas del ejercicio:**
1. ¿Cuál es el saldo medio positivo del conjunto de clientes?
2. ¿Entre qué valores se concentra el 50% de la edad de los clientes?
3. ¿Cuál es el saldo mediano por tipo de riesgo?
4. ¿Cuál es el nivel educativo más frecuente?
5. ¿Cómo es el sesgo de la distribución?
6. ¿Cómo es el sentido de la relación entre variables?

**Adaptación al dataset de bienes raíces:**
```python
# 1. Precio medio de propiedades
precio_medio = df['listPrice'].mean()
print(f"Precio medio: ${precio_medio:,.2f}")

# 2. Rango intercuartílico (IQR) - 50% central de sqft
q1 = df['sqft'].quantile(0.25)
q3 = df['sqft'].quantile(0.75)
print(f"50% central de sqft: {q1:.0f} - {q3:.0f}")

# 3. Precio mediano por tipo de propiedad
precio_por_tipo = df.groupby('type')['listPrice'].median()
print("\nPrecio mediano por tipo:")
print(precio_por_tipo)

# 4. Tipo de propiedad más frecuente
tipo_mas_frecuente = df['type'].mode()[0]
frecuencia = df['type'].value_counts().iloc[0]
print(f"\nTipo más frecuente: {tipo_mas_frecuente} ({frecuencia} casos)")

# 5. Sesgo de la distribución del precio
sesgo = df['listPrice'].skew()
print(f"\nAsimetría del precio: {sesgo:.4f}")
if sesgo > 0:
    print("  → Distribución asimétrica positiva (cola a la derecha)")
elif sesgo < 0:
    print("  → Distribución asimétrica negativa (cola a la izquierda)")
else:
    print("  → Distribución simétrica")

# 6. Relación entre variables (correlación)
corr_sqft_precio = df['sqft'].corr(df['listPrice'])
print(f"\nCorrelación sqft - listPrice: {corr_sqft_precio:.4f}")

if abs(corr_sqft_precio) > 0.7:
    sentido = "positiva" if corr_sqft_precio > 0 else "negativa"
    print(f"  → Relación lineal FUERTE {sentido}")
elif abs(corr_sqft_precio) > 0.3:
    sentido = "positiva" if corr_sqft_precio > 0 else "negativa"
    print(f"  → Relación lineal MODERADA {sentido}")
else:
    print(f"  → Relación lineal DÉBIL o inexistente")
```

---

## 📚 Estructura Sugerida del Notebook Final

```python
# ========================================
# TRABAJO PRÁCTICO INTEGRADOR
# Métodos Predictivos para la Gestión
# Dataset: Colorado Real Estate 2026
# ========================================

# ÍNDICE
# 1. Carga y Exploración Inicial
# 2. Limpieza de Datos
# 3. Análisis Descriptivo
# 4. Muestreo (n=750)
# 5. Modelo Estadístico - Muestra
# 6. Modelo Estadístico - Base Total
# 7. Comparación Estadística
# 8. Machine Learning
# 9. Conclusiones Comparativas

# ========================================
# SECCIÓN 1: CARGA Y EXPLORACIÓN INICIAL
# ========================================

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')

# Configuración de visualización
sns.set_theme(style='whitegrid', palette='tab10')
plt.rcParams['figure.dpi'] = 110

# Cargar dataset
df = pd.read_csv('colorado_real_estate_2026.csv')

# ... continuar con el resto del análisis
```

---

## ✅ Checklist de Entregables

- [ ] **Google Colab Notebook** autosuficiente y reproducible
- [ ] **Celdas de texto** explicando cada procedimiento y decisión
- [ ] **Gráficos con interpretación** inmediatamente después
- [ ] **i) Preprocesamiento:**
  - [ ] Identificación y tratamiento de valores faltantes
  - [ ] Detección de outliers (boxplots/Z-score)
  - [ ] Eliminación de duplicados
  - [ ] Análisis descriptivo completo
  - [ ] Matrices de correlación
- [ ] **ii) Muestreo:**
  - [ ] Extracción de muestra n=750
  - [ ] Justificación de representatividad
  - [ ] Comparación media/varianza muestra vs población
- [ ] **iii) Modelado Estadístico:**
  - [ ] Regresión Lineal/Logística con muestra (n=750)
  - [ ] Regresión Lineal/Logística con base total
  - [ ] Comparación crítica de coeficientes, p-valores y R²/Accuracy
  - [ ] Discusión sobre significatividad
- [ ] **iv) Machine Learning:**
  - [ ] División Train/Test
  - [ ] Mínimo 2 algoritmos (Decision Tree, Random Forest, k-NN, Gradient Boosting)
  - [ ] Evaluación con métricas apropiadas (MAE, MSE, RMSE, R² / Matriz confusión, Precision, Recall, F1, ROC-AUC)
  - [ ] Discusión sobre overfitting y control
- [ ] **v) Conclusión Comparativa:**
  - [ ] Modelo estadístico vs ML
  - [ ] Interpretabilidad vs capacidad predictiva
  - [ ] Recomendación para producción

---

## 👥 Información del Equipo

**Integrantes:**
1. [Nombre 1]
2. [Nombre 2]
3. [Nombre 3]
4. [Nombre 4]

**Roles sugeridos:**
- **Coordinador/a:** Supervisión general y integración
- **Análisis de Datos:** Preprocesamiento y descriptivas
- **Modelado Estadístico:** Regresión y análisis estadístico
- **Machine Learning:** Implementación de algoritmos y métricas

---

## 📞 Contacto y Colaboración

**Reuniones:** [Días/horarios acordados]  
**Herramientas de colaboración:** Google Drive, GitHub, Slack/Discord  
**Fecha de entrega:** [Completar]  
**Fecha de defensa oral:** [Completar]

---

## 📖 Referencias Adicionales

1. **Documentación Pandas:** https://pandas.pydata.org/docs/
2. **Documentación Statsmodels:** https://www.statsmodels.org/
3. **Documentación Scikit-learn:** https://scikit-learn.org/
4. **Seaborn Gallery:** https://seaborn.pydata.org/examples/index.html
5. **Interpretación de regresión:** https://www.statsmodels.org/stable/examples/index.html

---

**Última actualización:** Mayo 3, 2026  
**Versión:** 1.0

---

**Notas importantes:**
- Todos los archivos de referencia están en rutas relativas al proyecto
- Ejecutar notebooks en Google Colab para reproducibilidad
- Guardar versiones intermedias del trabajo
- Comentar el código de manera clara y profesional
- Utilizar markdown para explicaciones narrativas entre celdas de código
