# Feedback y recomendaciones para Taller 04

## Veredicto

El notebook `Taller_04.ipynb` cumple técnicamente con los requisitos principales del enunciado. Incluye generación de datos sintéticos, eliminación de outliers, exploración de `K=2..12`, métodos Silhouette y Elbow, evaluación mediante Ground Truth con ARI/NMI y documentación en celdas Markdown.

Sin embargo, todavía hay algunos aspectos que deben revisarse antes de realizar la entrega definitiva.

## Hallazgos importantes

### 1. Problema de seguimiento en Git

Git reconoce como eliminado el archivo:

```text
Taller 4/Taller_04.ipynb
```

Mientras que el notebook actual se encuentra en:

```text
taller 4/Taller_04.ipynb
```

La diferencia entre `Taller 4` y `taller 4` representa una ruta diferente. El archivo actual aparece como no rastreado, por lo que existe el riesgo de que el notebook correcto no sea incluido en GitHub.

Este es el punto más importante que debe verificarse antes de entregar.

### 2. LOF no elimina todos los outliers

En la sección de remoción de outliers se reporta:

- Outliers agregados: `150`.
- Puntos removidos por LOF: `150`.
- Outliers verdaderos restantes: `32`.

Esto significa que LOF eliminó aproximadamente 118 outliers verdaderos, pero también eliminó 32 puntos pertenecientes a los grupos legítimos. La detección tiene aproximadamente:

- Precisión: `78.7%`.
- Recall: `78.7%`.
- Outliers verdaderos no detectados: `32`.
- Puntos legítimos eliminados: `32`.

Por lo tanto, `X_clean` no está completamente limpio. La solución cumple con aplicar un método de remoción, pero las conclusiones no deberían afirmar que todos los outliers fueron eliminados.

### 3. Las métricas utilizan conjuntos diferentes

K-Means y Silhouette se ejecutan sobre `X_clean`, que todavía contiene 32 outliers verdaderos.

En cambio, ARI y NMI se calculan solamente sobre los puntos con etiquetas verdaderas distintas de `-1`. Esto deja 1168 puntos legítimos para la evaluación.

El procedimiento no es necesariamente inválido, pero debe explicarse claramente porque los outliers residuales influyen en los centroides antes de ser excluidos del cálculo de ARI y NMI.

### 4. La selección final de K necesita mejor justificación

Los resultados obtenidos son:

| Método | K seleccionado |
|---|---:|
| Silhouette | 5 |
| Elbow | 5 |
| Ground Truth - ARI | 6 |
| Ground Truth - NMI | 6 |
| Número real de grupos | 6 |

El modelo final utiliza `K=6` porque se conoce el número real de grupos generados. Esto es válido para datos sintéticos, pero debe indicarse explícitamente que los métodos no supervisados seleccionaron `K=5`, mientras que las métricas basadas en Ground Truth seleccionaron `K=6`.

En un escenario real sin etiquetas verdaderas, la selección debería justificarse principalmente con métodos no supervisados como Silhouette y Elbow.

### 5. Información de autores incompleta

El notebook todavía contiene:

```text
Autores: Completar nombres del grupo
```

Aunque los tres integrantes aparecen posteriormente, debe eliminarse el texto pendiente y dejar una identificación única y coherente del grupo. El número de integrantes sí cumple el máximo de tres personas establecido en el enunciado.

## Revisión de requisitos

| Requisito | Estado | Observación |
|---|---|---|
| Seis grupos gaussianos bidimensionales | Cumple | Se generan seis grupos con 200 puntos cada uno. |
| Distribuciones isotrópicas | Cumple | Se utiliza la misma desviación estándar en ambos ejes. |
| Igual número de puntos por grupo | Cumple | Cada grupo tiene 200 puntos. |
| Diferentes densidades | Cumple parcialmente | Hay diferentes desviaciones, aunque dos grupos comparten `sigma=0.7`. |
| Valores dentro de `[-10,10]` | Cumple | Se utiliza `np.clip` y se muestran los rangos obtenidos. |
| Dos grupos parcialmente superpuestos | Cumple | Los grupos 5 y 6 tienen centros cercanos. |
| Outliers uniformes | Cumple | Se generan 150 puntos uniformes en `[-10,10]`. |
| Remoción de outliers | Cumple con observación | Se aplica LOF, pero quedan 32 outliers verdaderos. |
| Exploración de `K=2..12` | Cumple | Se evalúan todos los valores del intervalo. |
| Método Silhouette | Cumple | Se calcula, grafica y selecciona el máximo. |
| Método del codo | Cumple | Se calcula mediante la distancia a la línea de referencia. |
| Método basado en Ground Truth | Cumple | Se utilizan ARI y NMI. |
| Código Python | Cumple | Se utilizan NumPy, Pandas, Matplotlib y Scikit-Learn. |
| Explicación en Markdown | Cumple | El notebook contiene explicaciones por sección. |
| Grupo de máximo tres personas | Cumple | Aparecen tres integrantes. |
| Repositorio compartido con el profesor | No verificable | Debe comprobarse directamente en GitHub. |
| Enlace enviado por Brightspace | No verificable | Debe confirmarse como parte de la entrega externa. |

## Mejoras recomendadas

1. Incluir una tabla con verdaderos positivos, falsos positivos, falsos negativos y outliers residuales de LOF.

2. Explicar en las conclusiones que LOF es un método no supervisado y que no recuperó perfectamente los outliers inyectados.

3. Aclarar que `K=6` fue seleccionado por Ground Truth y por el número conocido de grupos, no por consenso entre Silhouette y Elbow.

4. Explicar que la evaluación de ARI/NMI excluye los outliers verdaderos restantes, aunque estos sí participaron en el ajuste de K-Means.

5. Considerar separar claramente el conjunto usado para entrenar, el conjunto filtrado por LOF y el conjunto utilizado únicamente para evaluar con las etiquetas verdaderas.

6. Mostrar los outliers residuales con un color o marcador diferente en el gráfico final.

7. Utilizar seis desviaciones estándar distintas si se quiere demostrar de forma más clara la diferencia de densidad entre todos los grupos.

8. Evitar depender exclusivamente de `np.clip` para garantizar el rango, ya que el recorte puede modificar teóricamente la distribución gaussiana. Una alternativa más rigurosa sería generar los puntos mediante rechazo o utilizar una distribución gaussiana truncada.

9. Añadir un `assert` que compruebe explícitamente que todos los valores se encuentran dentro de `[-10,10]`.

10. Añadir un archivo de dependencias, como `requirements.txt`, para facilitar la reproducción del notebook.

11. Verificar que la carpeta y el notebook correctos estén incluidos en GitHub, especialmente la diferencia entre `Taller 4` y `taller 4`.

12. Completar correctamente los nombres de los autores antes de la entrega.

## Elementos que no faltan

El enunciado no exige incluir todas las secciones adicionales del notebook de ejemplo. Por tanto, no es obligatorio agregar:

- PCA.
- Verificación adicional de esfericidad.
- Matriz de confusión.
- Accuracy, precision, recall o F1-score.
- Implementación de K-Means desde cero.

ARI y NMI son métodos válidos para cumplir el requisito de evaluación basada en Ground Truth.

## Estado de ejecución

La revisión estática muestra que las 12 celdas de código tienen resultados almacenados y que no hay salidas de error en el notebook. Además, el código de las celdas es sintácticamente válido.

No se realizó una ejecución independiente en el entorno actual porque no están instaladas las dependencias `numpy`, `pandas`, `matplotlib` y `scikit-learn`. Por ello, los resultados numéricos fueron revisados a partir de las salidas guardadas en el notebook.

## Conclusión final

La solución es adecuada y cubre los requisitos académicos principales. Antes de entregarla, se recomienda revisar especialmente el seguimiento del archivo en Git, corregir la información de los autores y documentar la detección incompleta de outliers y la diferencia entre los valores de K seleccionados por cada método.
