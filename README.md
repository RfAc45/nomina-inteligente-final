# Nómina Inteligente

**Proyecto final · CIA6041 Inteligencia Artificial y Gestión de Datos Organizacionales · Broward International University**
**Caso:** Advanced Tech Group (empresa ficticia del sector tecnológico)

## Integrantes (Grupo 1)

- Jesús Jaquez González
- Libia Ortiz Bedoya
- Roberto Francisco Armuelles Contreras
- Sergio Sebastián Rivera Flores

## Pregunta de negocio

**¿Qué empleados tienen mayor riesgo de renunciar, y qué factores (salario, horas extra, antigüedad) influyen más en esa decisión?**

Está dirigida al equipo de Recursos Humanos y a la dirección financiera, para planificar el presupuesto de retención de talento.

## Datos

- **Dataset:** IBM HR Analytics Employee Attrition & Performance (1,470 empleados, 35 variables). Es un dataset público y ficticio.
- **Fuente:** https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
- **Licencia:** Database Contents License (DbCL) v1.0.
- **Limpieza:** se eliminaron 3 columnas constantes (`EmployeeCount`, `Over18`, `StandardHours`) y no se eliminó ninguna fila (0% de valores vacíos, 0 duplicados). `EmployeeNumber` se conserva como identificador, pero se excluye de los modelos porque incluirlo produce fuga de datos. Se crearon `Banda salarial`, `Grupo de antigüedad` y `CandidatoPromocion`.

## Estructura del repositorio

```
├── README.md
├── data/
│   ├── raw/WA_Fn-UseC_-HR-Employee-Attrition.csv   # dataset original de Kaggle
│   ├── dataset_nomina_limpio.csv                    # salida del notebook 1 (35 columnas)
│   └── dataset_nomina_segmentos.csv                 # salida del notebook 2 (agrega Segmento)
├── notebooks/
│   ├── CIA6041_NominaInteligente_ETL_EDA.ipynb      # 1. limpieza, indicadores y gráficos
│   └── CIA6041_NominaInteligente_Modelos.ipynb      # 2. árbol de decisión y K-Means
├── dashboard/
│   ├── CIA6041_NominaInteligente_Dashboard_Final.pbix   # dashboard principal (Power BI)
│   ├── CIA6041_NominaInteligente_Tableau.twbx           # versión en Tableau (comparación)
│   └── Capturas_Dashboard.pdf                           # capturas de las vistas principales
└── docs/
    ├── Informe_Tecnico_Final.pdf
    └── Guia_Modelos_Semana8.html                    # guía paso a paso (descargar y abrir en el navegador)
```

## Cómo ejecutar el proyecto

Los notebooks se ejecutan en Google Colab (o en Jupyter) y deben correrse **en orden**.

**Notebook 1: ETL y EDA**
1. En [colab.research.google.com](https://colab.research.google.com), abre `notebooks/CIA6041_NominaInteligente_ETL_EDA.ipynb` con Archivo → Subir cuaderno.
2. Sube `data/raw/WA_Fn-UseC_-HR-Employee-Attrition.csv` al panel de archivos de Colab, junto al notebook.
3. Ejecuta todas las celdas (Entorno de ejecución → Ejecutar todas).
4. Descarga el archivo que genera: `dataset_nomina_limpio.csv`.

**Notebook 2: Modelos**
1. Abre `notebooks/CIA6041_NominaInteligente_Modelos.ipynb` en una sesión nueva de Colab.
2. Sube `dataset_nomina_limpio.csv` al panel de archivos, en la misma ubicación que el notebook.
3. Ejecuta todas las celdas. Genera `dataset_nomina_segmentos.csv` y los gráficos.

Todos los modelos usan `random_state=42`, por lo que los resultados son reproducibles. Los notebooks leen los CSV desde la misma carpeta en la que están, así que si se ejecutan fuera de Colab hay que copiar el CSV junto al notebook.

**Dashboard**
- `.pbix`: requiere Power BI Desktop (solo Windows). Si no se puede abrir, `Capturas_Dashboard.pdf` muestra las vistas principales.
- `.twbx`: se abre con Tableau Desktop o Tableau Public.

## Resultados principales

| Indicador | Valor |
|---|---|
| Tasa de rotación | 16.12% (237 de 1,470) |
| Costo total de nómina | $9,559,309 / mes |
| Salario promedio | $6,502.93 |
| Factor de riesgo principal | Horas extra: 2.93 veces más probabilidad de renuncia |

**Árbol de decisión (predice `Attrition`), comparado contra la línea base**

| Métrica | Línea base (predecir siempre "No") | Árbol de decisión |
|---|---|---|
| Exactitud | 83.90% | 73.24% |
| Precisión (renuncia) | — | 32.06% |
| Sensibilidad (renuncia) | — | 59.15% |
| F1 (renuncia) | — | 41.58% |

La exactitud queda por debajo de la línea base por el desbalance de clases (solo 16% renuncia). El modelo cambia exactitud general por detectar más renuncias reales.

**K-Means (segmentación, k=3, índice de silueta 0.623)**

| Segmento | Empleados | Salario promedio | Antigüedad | Tasa de renuncia |
|---|---|---|---|---|
| Junior | 916 | ~$3,600 | ~5 años | 19.5% |
| Medio | 365 | ~$8,500 | ~8 años | 13.2% |
| Senior | 189 | ~$16,700 | ~14 años | 5.3% |

## Limitaciones

- El dataset es sintético y de un solo corte en el tiempo, por lo que no permite validar los modelos contra renuncias reales.
- El árbol no tiene validación cruzada ni ajuste de hiperparámetros.
- El valor de k=3 se justifica con el método del codo y criterio de negocio, no con un óptimo único.
- No se recomienda poner estos modelos en producción sin esas mejoras. El detalle está en `docs/Informe_Tecnico_Final.pdf`.

## Uso de herramientas de IA

Se usó asistencia de inteligencia artificial (Claude, de Anthropic) para organizar documentos, estructurar código y redactar guiones. El equipo revisó y verificó el contenido contra los datos reales antes de incorporarlo.
