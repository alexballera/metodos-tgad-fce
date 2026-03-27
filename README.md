# Métodos Predictivos para la Gestión

> **Universidad de Buenos Aires — Facultad de Ciencias Económicas**  
> Tecnicatura en Gestión de Datos (TGAD) | Cátedra Bianco

---

## ¿De qué trata esta materia?

**Métodos Predictivos para la Gestión** es una materia del segundo año de la Tecnicatura en Gestión de Datos. Su objetivo es que los alumnos dominen las herramientas estadísticas y computacionales para analizar datos, construir modelos predictivos y apoyar la toma de decisiones en organizaciones.

El enfoque es **práctico**: cada concepto estadístico se implementa en Python con datos reales o inspirados en casos del mundo de los negocios (fintech, e-commerce, telecomunicaciones, salud).

---

## Estructura del repositorio

```
metodos-tgad-fce/
├── fuentes/                        # Datasets y fuentes de datos
└── sesiones/
    ├── u1/                         # Unidad 1: Inferencia y Bootstrapping
    │   ├── clase1/                 # PDFs: muestreo, fórmulas, TPs
    │   ├── clase2/                 # Notebooks: Inferencia clásica y Bootstrapping
    │   └── ejercicios/             # TPs para resolver
    └── u2/                         # Unidad 2: Pretratamiento de Datos
        └── U2-Pretratamiento de Datos.pdf
```

---

## Contenido del programa

### Unidad 1 — Inferencia Estadística y Bootstrapping

La primera unidad cubre las técnicas clásicas de inferencia estadística y las contrasta con el enfoque computacional de bootstrapping.

| Tema | Archivo |
|------|---------|
| Métodos de muestreo | [clase1/U1_Metodos_de_Muestreo_e_IE.pdf](sesiones/u1/clase1/U1_Metodos_de_Muestreo_e_IE.pdf) |
| Fórmulas de referencia (IC para media, varianza, proporción) | [clase1/U1_Formulas_de_inferencia.pdf](sesiones/u1/clase1/U1_Formulas_de_inferencia.pdf) |
| Notebook: Inferencia clásica en Python | [clase2/U1-Inferencia.html](sesiones/u1/clase2/U1-Inferencia.html) |
| Notebook: Inferencia vs. Bootstrapping | [clase2/U1-Inferencia vs Bootstrapping.html](<sesiones/u1/clase2/U1-Inferencia vs Bootstrapping.html>) |
| TP: Estrategias de muestreo | [ejercicios/U1-TP_Estrategias-de-Muestreo.pdf](sesiones/u1/ejercicios/U1-TP_Estrategias-de-Muestreo.pdf) |
| TP: Ejercicios sobre intervalos de estimación | [ejercicios/U1-TP_Ejercicios-sobre-IE.pdf](sesiones/u1/ejercicios/U1-TP_Ejercicios-sobre-IE.pdf) |

**Temas cubiertos en los notebooks:**

- Intervalos de confianza para la media (σ conocido y desconocido)
- Test t de Student (una y dos colas)
- Intervalo de confianza para la varianza (distribución χ²)
- Intervalo de confianza para proporciones y diferencias de proporciones
- Determinación del tamaño de muestra
- Bootstrapping: remuestreo con reemplazo e IC percentil
- Comparación entre inferencia clásica y bootstrapping

---

### Unidad 2 — Pretratamiento de Datos

| Tema | Archivo |
|------|---------|
| Pretratamiento de datos con pandas | [u2/U2-Pretratamiento de Datos.pdf](<sesiones/u2/U2-Pretratamiento de Datos.pdf>) |

---

## Stack tecnológico

Los notebooks están desarrollados en **Python** (Jupyter). Las librerías principales son:

```python
import numpy as np
import pandas as pd
from scipy import stats
import statsmodels.stats.api as sms
import matplotlib.pyplot as plt
from ucimlrepo import fetch_ucirepo  # para datasets del repositorio UCI ML
```

---

## Cómo usar este repositorio

### Ver los notebooks

Los notebooks de clase están exportados como archivos `.html`. Podés abrirlos directamente en el navegador sin instalar nada:

```
sesiones/u1/clase2/U1-Inferencia.html
sesiones/u1/clase2/U1-Inferencia vs Bootstrapping.html
```

### Ejecutar los notebooks localmente

1. Cloná el repositorio:
   ```bash
   git clone <url-del-repo>
   cd metodos-tgad-fce
   ```

2. Creá un entorno virtual e instalá las dependencias:
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   pip install numpy pandas scipy statsmodels matplotlib ucimlrepo jupyter
   ```

3. Iniciá Jupyter:
   ```bash
   jupyter notebook
   ```

---

## Tutor virtual con GitHub Copilot

Este repositorio incluye un **agente tutor** para GitHub Copilot que puede ayudarte con dudas sobre el material, ejercicios y código.

Para usarlo en VS Code:

1. Abrí el panel de chat de GitHub Copilot (`Ctrl+Alt+I` o `Cmd+Alt+I`).
2. Seleccioná el agente **Tutor Bianco** desde el selector de agentes.
3. Hacé tu consulta en lenguaje natural, por ejemplo:
   - *"¿Cómo calculo un intervalo de confianza para la media con scipy?"*
   - *"Explicame la diferencia entre inferencia clásica y bootstrapping"*
   - *"Ayudame a resolver el ejercicio 3 del TP de muestreo"*

También podés invocar el skill de tutoría escribiendo `/tutor-profesor` en el chat.

---

## Contacto y consultas académicas

Para consultas formales dirigirse a la cátedra a través de los canales oficiales de la Facultad de Ciencias Económicas, UBA.
