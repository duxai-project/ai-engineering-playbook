# 📊 Estadística básica para Data Analysis (Manual personal)

## 1. ¿Qué es una distribución?
Una distribución muestra **cómo se reparten los valores** de una variable.

Sirve para entender:
- dónde está la mayoría de los datos
- si hay valores extremos
- si los datos son simétricos o sesgados

Ejemplos de gráficos:
- histogram
- KDE
- boxplot

👉 Pregunta que responde:
> ¿Cómo se comporta esta variable por sí sola?

---

## 2. Mean vs Median
- **Mean (promedio)**
  (v1 + v2 + ... + vn) / n
  Sensible a valores extremos.

- **Median (mediana)**
  Valor central de los datos ordenados.
  Más robusta frente a outliers.

👉 Regla práctica:
- datos normales → mean
- datos con outliers → median

---

## 3. Variabilidad
La variabilidad indica **qué tan dispersos** están los datos.

Medidas comunes:
- desviación estándar (std)
- rango intercuartílico (IQR)

Alta variabilidad:
- datos muy distintos entre sí

Baja variabilidad:
- datos más parecidos

👉 Pregunta que responde:
> ¿Qué tan desparejos son los valores?

---

## 4. Outliers
Outliers son **valores extremos** comparados con el resto.

- No siempre son errores
- Pueden afectar mucho al promedio
- Se detectan bien con boxplots

👉 Pregunta que responde:
> ¿Hay valores raros que distorsionan el análisis?

---

## 5. Correlación ≠ Causalidad
Correlación:
- dos variables se mueven juntas

Causalidad:
- una variable provoca cambios en la otra

⚠️ Ver correlación NO implica causa.

👉 Ejemplo:
- gasto ↑ y propina ↑ → correlación
- no prueba que gastar más cause más propina

---

## 6. ¿Qué pregunta responde cada gráfico?

### Distribution plots
(1 variable)
- ¿Cómo se distribuyen los valores?

### Relationship plots
(2 variables)
- ¿Cómo se relacionan entre sí?

### Categorical plots
(categoría + número)
- ¿Cómo cambia una métrica según grupos?

### Barplot
- muestra un resumen estadístico (mean, std, etc.)
- NO muestra datos crudos

### Boxplot
- muestra la distribución real resumida
- mediana, dispersión y outliers
- sin estimaciones ocultas

---

## 7. Skewness y Kurtosis (muy corto)
- **Skewness**: sesgo de la distribución (cola hacia la derecha o izquierda)
- **Kurtosis**: qué tan “picuda” o “achatada” es respecto a una normal

👉 Regla rápida:
- skewness > 0 → cola a la derecha
- skewness < 0 → cola a la izquierda

---

## 8. Z-score y estandarización
**Z-score**: cuántas desviaciones estándar está un valor de la media.  
Fórmula: z = (x - mean) / std

👉 Se usa para:
- comparar valores en distintas escalas
- detectar outliers (|z| > 3 es sospechoso)

---

## 9. Normal vs no normal (atajo para elegir resumen)
- Si es **aprox. normal**: reportar `mean ± std`
- Si hay **outliers o sesgo fuerte**: usar `median + IQR`

---

## 10. Correlación vs Causalidad (entrevista)
**Correlación**: dos variables se mueven juntas.  
**Causalidad**: una variable provoca cambios en la otra.

👉 Frases rápidas:
- “Puede haber correlación sin causalidad.”
- “Para causalidad necesito diseño experimental o buen control de confusores.”

---

## 11. Checklist rápido antes de interpretar
- ¿Qué variable(s) estoy viendo?
- ¿Es una sola variable o relación entre dos?
- ¿Hay outliers o sesgo fuerte?
- ¿Qué medida de resumen es más confiable?

---

## Regla de oro
Antes de interpretar un gráfico, preguntarse:
> ¿Qué variable estoy viendo y qué pregunta intenta responder este gráfico?
