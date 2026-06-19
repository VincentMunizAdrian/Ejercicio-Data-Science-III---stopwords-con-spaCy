# Ejercicio-Data-Science-III---stopwords-con-spaCy

# Práctica: Implementación y evaluación de eliminación de stopwords con spaCy

## Descripción

En esta práctica se aplicaron técnicas de Procesamiento de Lenguaje Natural (NLP) utilizando la biblioteca **spaCy** para realizar la tokenización y eliminación de stopwords sobre un conjunto de reseñas de productos de Amazon.

Además, se implementó una lista personalizada de stopwords para adaptar la limpieza al dominio específico del dataset y se analizaron los cambios producidos en la frecuencia de términos antes y después del preprocesamiento.

---

# Objetivos

- Aplicar tokenización utilizando spaCy.
- Eliminar stopwords estándar del idioma inglés.
- Personalizar la lista de stopwords según el contexto del dataset.
- Comparar la frecuencia de palabras antes y después de la limpieza.
- Evaluar el impacto del preprocesamiento en la representación textual de las reseñas.

---

# Dataset

Se utilizó el dataset **Amazon Reviews Dataset** disponible en Kaggle:

https://www.kaggle.com/datasets/dongrelaxman/amazon-reviews-dataset

El conjunto de datos contiene reseñas realizadas por usuarios sobre productos comercializados en Amazon.

### Columnas principales

| Columna | Descripción |
|----------|------------|
| Reviewer Name | Nombre del usuario |
| Country | País del usuario |
| Rating | Calificación otorgada |
| Review Title | Título de la reseña |
| Review Text | Texto completo de la reseña |
| Date of Experience | Fecha de la experiencia |

Dimensiones del dataset:

```text
(21214, 9)
```

---

# Herramientas utilizadas

- Python 3
- Pandas
- NumPy
- spaCy
- Google Colab

Modelo lingüístico utilizado:

```python
en_core_web_sm
```

> Inicialmente se utilizó el modelo `es_core_news_sm`, pero debido a que las reseñas se encontraban en idioma inglés se reemplazó por `en_core_web_sm`, permitiendo una correcta identificación de stopwords y un procesamiento más preciso del texto.

---

# Metodología

## 1. Carga y exploración de datos

Se cargó el archivo CSV utilizando Pandas y se realizó una inspección inicial para verificar:

- Cantidad de registros.
- Columnas disponibles.
- Calidad de los datos.
- Presencia de valores nulos.

Posteriormente se seleccionó la columna **Review Text** para realizar el análisis de lenguaje natural.

---

## 2. Tokenización

Se utilizó spaCy para dividir cada reseña en tokens individuales.

### Ejemplo

Texto original:

```text
This product is really good and easy to use.
```

Tokens generados:

```python
['This', 'product', 'is', 'really', 'good', 'and', 'easy', 'to', 'use', '.']
```

La tokenización permite transformar el texto libre en unidades analizables por algoritmos de NLP.

---

## 3. Eliminación de stopwords estándar

Se eliminaron las stopwords incluidas por defecto en spaCy para el idioma inglés.

Algunos ejemplos de stopwords eliminadas:

```text
the
and
is
to
of
a
i
they
```

Estas palabras son extremadamente frecuentes y aportan poca información semántica para tareas analíticas.

---

## 4. Personalización de stopwords

Tras analizar las palabras más frecuentes obtenidas después de la limpieza estándar, se incorporaron nuevas stopwords específicas del dominio:

```python
custom_stopwords = {
    "product",
    "amazon",
    "buy",
    "bought",
    "service"
}
```

### Justificación

Estas palabras aparecen frecuentemente en las reseñas debido al contexto específico del dataset, pero no aportan información significativa sobre la opinión o experiencia del usuario.

Por ejemplo:

- **product** aparece en gran cantidad de reseñas.
- **amazon** hace referencia a la plataforma.
- **buy** y **bought** describen la acción de compra.

La eliminación de estos términos permite resaltar conceptos más relevantes relacionados con la calidad, servicio y experiencia del cliente.

---

# Comparación de resultados

## Antes de eliminar stopwords

Las palabras más frecuentes correspondían principalmente a términos funcionales del idioma inglés:

| Palabra | Frecuencia |
|----------|------------|
| i | 3138 |
| the | 2912 |
| to | 2745 |
| and | 2367 |
| a | 1807 |

---

## Después de eliminar stopwords estándar

Comenzaron a destacarse términos relacionados con las reseñas:

| Palabra | Frecuencia |
|----------|------------|
| amazon | 1465 |
| customer | 529 |
| service | 513 |
| delivery | 400 |
| order | 351 |

---

## Después de eliminar stopwords personalizadas

La representación textual se volvió más específica y descriptiva:

| Palabra | Frecuencia |
|----------|------------|
| customer | 529 |
| delivery | 400 |
| order | 351 |
| time | 293 |
| item | 283 |

---

# Resultados

La eliminación de stopwords permitió:

- Reducir la cantidad de términos procesados.
- Eliminar ruido lingüístico.
- Mejorar la interpretación de las palabras más relevantes.
- Facilitar futuras tareas de clasificación y análisis de sentimientos.

La personalización de stopwords mejoró aún más la representación del texto al eliminar términos demasiado frecuentes dentro del contexto específico de Amazon.

---

# Reflexión

La limpieza de texto constituye una etapa fundamental dentro de cualquier proyecto de Procesamiento de Lenguaje Natural.

Durante la práctica se observó que las stopwords estándar eliminan palabras funcionales del idioma, mientras que las stopwords personalizadas permiten adaptar el análisis al dominio específico del problema.

Los resultados obtenidos muestran que una adecuada estrategia de preprocesamiento mejora significativamente la calidad de la información disponible para posteriores análisis, como clasificación automática de reseñas o análisis de sentimientos.

---

## Diversidad Léxica

Para evaluar la riqueza del vocabulario presente en el corpus se calculó la **diversidad léxica**, definida como:

\[
\text{Diversidad Léxica} =
\frac{\text{Número de palabras únicas}}
{\text{Número total de palabras}}
\]

Esta métrica permite medir qué tan variado es el vocabulario utilizado en los textos. Valores más altos indican una mayor proporción de términos únicos respecto al total de palabras procesadas.

### Resultados obtenidos

| Etapa | Diversidad Léxica |
|---------|---------:|
| Texto original | 0.0696 |
| Stopwords estándar | 0.1577 |
| Stopwords personalizadas | 0.1687 |

### Interpretación

Los resultados muestran un incremento significativo de la diversidad léxica después de aplicar la limpieza del texto.

- En el **texto original**, la diversidad léxica es menor debido a la alta frecuencia de palabras funcionales del idioma (artículos, pronombres, preposiciones, etc.).
- Tras eliminar las **stopwords estándar**, la diversidad aumenta considerablemente, ya que desaparecen términos muy frecuentes que aportan poco contenido semántico.
- Finalmente, al incorporar **stopwords personalizadas** específicas del dominio de Amazon, la diversidad léxica vuelve a incrementarse, permitiendo destacar palabras más representativas de las opiniones y experiencias de los usuarios.

Estos resultados indican que la eliminación de stopwords mejora la calidad de la representación textual y facilita la identificación de términos relevantes para tareas posteriores de análisis de texto y procesamiento de lenguaje natural.

---

# Conclusiones

- spaCy permite realizar tokenización y limpieza de texto de manera eficiente.
- La eliminación de stopwords mejora la representación textual.
- Las stopwords personalizadas ayudan a adaptar el análisis al contexto del dataset.
- El preprocesamiento reduce ruido y facilita la extracción de información relevante.
- Una correcta selección del modelo lingüístico es fundamental para obtener resultados consistentes.

---

# Referencias

- spaCy Documentation: https://spacy.io/
- Pandas Documentation: https://pandas.pydata.org/
- Amazon Reviews Dataset: https://www.kaggle.com/datasets/dongrelaxman/amazon-reviews-dataset
