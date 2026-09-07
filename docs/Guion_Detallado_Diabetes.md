# Guion Detallado de Presentación
## "Análisis de Factores Demográficos, Clínicos y de Estilo de Vida en la Predicción del Riesgo de Diabetes"
### Universidad del Rosario — Análisis Estadístico de Datos

> **Nota de uso:** Este guion está pensado para ser leído y adaptado, no memorizado al pie de la letra. Las frases en *cursiva* son sugerencias de transición. Los tiempos son aproximados. Total estimado: **18–22 minutos** sin preguntas.

---

## SLIDE 1 — PORTADA
**⏱ ~1 minuto**

Buenos días / buenas tardes a todos. Mi nombre es [nombre], y junto con mis compañeros Samuel Arandia, Juan David Castañeda y Juan Felipe Rojas, les vamos a presentar el trabajo que desarrollamos para la asignatura Análisis Estadístico de Datos, bajo la dirección de la profesora Luz Adriana Pineda.

El título es: *"Análisis de Factores Demográficos, Clínicos y de Estilo de Vida en la Predicción del Riesgo de Diabetes"*, y los datos que usamos provienen de la encuesta NHANES 2013-2014 de los CDC.

*Vamos a la agenda para que tengan claridad sobre el recorrido.*

---

## SLIDE 2 — AGENDA
**⏱ ~30 segundos**

La presentación tiene seis bloques. Empezamos con la motivación y pregunta de investigación, luego describimos los datos y la metodología, después entramos en detalle al análisis exploratorio —que fue una etapa fundamental del trabajo— pasamos a la modelación estadística multivariada, presentamos los modelos predictivos con sus resultados, y cerramos con conclusiones.

---

## SLIDE 3 — INTRODUCCIÓN Y PREGUNTA DE INVESTIGACIÓN
**⏱ ~2.5 minutos**

La diabetes mellitus tipo 2 es una enfermedad crónica con una prevalencia creciente a nivel mundial. Su diagnóstico convencional depende de pruebas de laboratorio especializadas como la hemoglobina glicosilada, la glucosa en ayunas o la prueba de tolerancia oral a la glucosa. Estas pruebas son costosas, requieren tiempo y no siempre están disponibles en una primera consulta, lo que puede retrasar la detección oportuna.

Esto nos lleva a una pregunta práctica muy relevante: ¿podemos usar información que ya tenemos del paciente desde el primer contacto —su edad, su peso, su nivel de actividad física, su alimentación— para orientar qué exámenes pedir y anticipar quién tiene mayor riesgo?

Esa es exactamente nuestra **pregunta de investigación**:

> *¿En qué medida variables básicas —demográficas, medidas corporales, actividad física y dieta— permiten orientar la selección de exámenes clínicos relevantes y, a partir de estos, estimar la probabilidad de padecer diabetes?*

Para responderla, definimos tres objetivos específicos que estructuran el trabajo:

- Analizar la distribución de variables clínicas como HbA1c, presión arterial e IMC.
- Aplicar técnicas de probabilidad multivariada para modelar la relación conjunta entre las variables.
- Formular conclusiones que permitan optimizar la selección de exámenes clínicos hacia una detección más eficiente.

*Con eso claro, hablemos de dónde vienen los datos.*

---

## SLIDE 4 — DATOS Y MARCO METODOLÓGICO
**⏱ ~2 minutos**

Utilizamos la base de datos **NHANES 2013-2014** —National Health and Nutrition Examination Survey—, recolectada por los CDC de Estados Unidos y obtenida a través de Kaggle. Esta encuesta combina entrevistas, exámenes físicos y pruebas de laboratorio sobre una muestra representativa de la población estadounidense.

La base original es enorme: **10.175 individuos y 1.829 variables**. Después de un proceso riguroso de selección y depuración, nos quedamos con **16 variables relevantes** para los objetivos del estudio.

Un criterio de selección muestral importante: restringimos el análisis a **personas mayores de 45 años**, quedando con **3.174 individuos**. Esta decisión fue deliberada: la diabetes tipo 2 tiene una prevalencia mucho mayor en esta franja de edad, lo que hace que los patrones estadísticos sean más detectables y que el análisis sea más pertinente desde el punto de vista clínico.

Las variables se dividen en tres tipos:

- **Cuantitativas continuas** (escala de razón): edad, IMC, presión arterial sistólica y diastólica, colesterol total, hemoglobina glicosilada, insulina en sangre, consumo de carbohidratos y consumo de fibra.
- **Categóricas nominales**: género, etnicidad, nivel de actividad física, hábito tabáquico y diagnóstico de diabetes —que es nuestra variable respuesta.
- **Ordinal**: nivel educativo.

*Ahora sí, entremos al análisis exploratorio, que fue una etapa clave.*

---

## SLIDE 5 — ANÁLISIS EXPLORATORIO: VARIABLES CATEGÓRICAS
**⏱ ~3 minutos**

El análisis exploratorio no fue un paso decorativo. Fue lo que nos permitió entender la estructura de los datos antes de modelar, detectar problemas, y formular hipótesis informadas.

Empecemos con las **variables categóricas**.

**Diagnóstico de diabetes:** La distribución de la variable respuesta muestra que el **76.2% de la muestra no tiene diabetes**, el **19.5% tiene diagnóstico confirmado**, y el **4.2% está en condición borderline** —es decir, prediabetes. Las categorías "Refused" y "Don't know" tienen una participación prácticamente nula y no afectan el análisis.

Este desbalance es importante. Significa que si un modelo predijera "no diabetes" para todos, tendría una exactitud aparente del 76%, lo que podría parecer bueno pero sería completamente inútil. Por eso, a la hora de evaluar los modelos, no usamos solo la exactitud: priorizamos métricas como el AUC y la *balanced accuracy*, que son más robustas frente al desbalance de clases.

**Actividad física:** Hay una clara dominancia del sedentarismo en la muestra. La mayoría de individuos reporta no realizar actividad física de forma regular. Esto es relevante porque el sedentarismo es uno de los factores de riesgo modificables más consistentemente documentados para la diabetes tipo 2.

**Etnicidad:** La muestra presenta una alta concentración en población hispana, lo que representa una limitación. Este sesgo muestral implica que los resultados no necesariamente se generalizan a poblaciones más heterogéneas. Lo tuvimos en cuenta al interpretar los hallazgos.

*Ahora pasemos a las variables cuantitativas, donde los hallazgos son especialmente ricos.*

---

## SLIDE 5B — ANÁLISIS EXPLORATORIO: VARIABLES CUANTITATIVAS
**⏱ ~3.5 minutos**

*(Esta sección se expone verbalmente como continuación del exploratorio, aunque corresponde al mismo slide o al siguiente en la presentación.)*

Para las **variables cuantitativas**, realizamos análisis de tendencia central, variabilidad, posición y forma de distribución. El hallazgo general es que las variables presentan medias cercanas a sus medianas, lo que sugiere distribuciones relativamente simétricas en el centro. Sin embargo, hay diferencias importantes en dispersión y en la presencia de valores atípicos, según la variable.

Destaquemos las más importantes:

**Hemoglobina glicosilada (HbA1c):** Esta es la variable clínica más relevante del estudio, y su distribución lo evidencia. Presenta una **asimetría positiva marcada**: la mayoría de los valores se concentran en el rango de normalidad, pero hay una cola hacia la derecha con valores que superan el umbral diagnóstico de diabetes. Lo que llama la atención es que los valores centrales de la muestra están muy cerca del umbral de prediabetes, lo que refleja que estamos trabajando con una población de riesgo real. Además, tiene una **dispersión moderada**, lo que la convierte en una variable estable y de alto poder discriminatorio —dos características ideales para un predictor diagnóstico.

**Colesterol total:** La distribución es relativamente simétrica, con niveles medios dentro de rangos esperados para la población, aunque con algunos valores atípicos hacia niveles altos que corresponden a individuos con dislipidemia. Su relevancia clínica radica en que el colesterol es un marcador del perfil metabólico y cardiovascular asociado a la resistencia a la insulina.

**Insulina, carbohidratos y fibra:** Estas tres variables comparten un patrón de **alta asimetría positiva y alta variabilidad**. Hay individuos con consumos o niveles extremadamente altos, lo que genera colas largas hacia la derecha. Esto refleja la heterogeneidad real en los hábitos de la población: hay personas con dietas muy diferentes entre sí, y condiciones fisiológicas muy distintas en cuanto a producción de insulina. Esta variabilidad es un desafío estadístico pero también una fuente de información.

**IMC:** Los boxplots estratificados por diagnóstico de diabetes muestran una diferencia clara: los individuos con diagnóstico positivo presentan en promedio un IMC más alto que los no diabéticos. Aunque hay traslape entre grupos, la tendencia es consistente con la literatura y refuerza el rol del IMC como factor de riesgo.

**Presión arterial:** También muestra diferencias entre grupos, aunque más moderadas que el IMC y la HbA1c. La hipertensión coexiste frecuentemente con la diabetes como parte del síndrome metabólico, lo que le da relevancia en el análisis conjunto.

En términos generales, la presencia de asimetría, valores atípicos y la concentración de variables clínicas cerca del umbral diagnóstico confirman que estamos ante una población estadísticamente interesante y clínicamente pertinente.

*Con esta comprensión de los datos, pasemos a la etapa de modelación.*

---

## SLIDE 6 — MODELACIÓN ESTADÍSTICA MULTIVARIADA
**⏱ ~4 minutos**

La estrategia metodológica la estructuramos en cuatro etapas progresivas, desde el análisis descriptivo hasta la modelación predictiva.

### Etapa 1: MANOVA

Lo primero que quisimos establecer fue si, en términos estadísticos, los individuos con y sin riesgo de diabetes son poblaciones distintas cuando consideramos múltiples variables al mismo tiempo.

Para eso usamos un **MANOVA** —Análisis Multivariante de la Varianza. A diferencia de hacer varios ANOVA independientes, el MANOVA evalúa si los grupos difieren en una combinación lineal de variables de forma simultánea, lo que captura mejor la realidad multidimensional del fenómeno.

El resultado fue contundente: **los perfiles multivariados de los grupos con y sin riesgo son significativamente diferentes**. Esto valida el enfoque multivariado del estudio y confirma que tiene sentido buscar patrones conjuntos.

### Etapa 2: Análisis Factorial con Rotación Oblimin

Esta fue una de las etapas más importantes y conceptualmente más ricas del análisis.

Aplicamos un **Análisis Factorial Exploratorio** para identificar la estructura latente de las variables —es decir, para descubrir qué variables comparten información subyacente y se pueden agrupar en factores comunes.

Una decisión metodológica clave aquí fue el **tipo de rotación**. Elegimos la **rotación oblimin**, que es una rotación oblicua. Esto significa que permitimos que los factores resultantes estén correlacionados entre sí.

¿Por qué oblimin y no una rotación ortogonal como varimax? Porque la rotación ortogonal asume que los factores son independientes —que los constructos subyacentes no tienen relación entre sí. Y eso es un supuesto que simplemente no se sostiene en fisiología humana.

En el cuerpo humano, los constructos **dietarios, demográficos y metabólicos no operan de forma independiente**. Son naturalmente correlacionados: una dieta alta en carbohidratos refinados afecta la insulina, que afecta el IMC, que afecta la presión arterial, que se relaciona con el colesterol. Forzar independencia entre estos factores habría producido una solución factorial artificialmente limpia, pero estadísticamente inapropiada para la naturaleza del fenómeno.

La rotación oblimin nos permitió recuperar esa correlación natural entre constructos y obtener una solución más fiel a la realidad biológica.

El resultado fue que la variabilidad de los datos se organiza en **tres dimensiones latentes**:

1. **Factor dietario**: agrupó variables de consumo alimentario como carbohidratos y fibra. Refleja los patrones de alimentación de la población.

2. **Factor cardiovascular-demográfico**: agrupó variables como presión arterial, edad e IMC. Representa el perfil físico y las condiciones del sistema cardiovascular.

3. **Factor clínico-metabólico**: agrupó hemoglobina glicosilada, insulina y colesterol. Es el factor más directamente asociado al control glicémico y el riesgo de diabetes.

Este resultado tiene una implicación importante: **el riesgo de diabetes no puede explicarse desde una sola dimensión**. Intervienen simultáneamente patrones de alimentación, condiciones físicas y marcadores metabólicos. Ninguno de los tres factores, por separado, cuenta toda la historia.

El ACP complementario mostró consistencia con estos tres ejes de variación, confirmando la robustez de la estructura factorial.

### Etapa 3: ANOVA / ANCOVA

Complementamos el análisis con pruebas de hipótesis estadísticas —ANOVA y ANCOVA según correspondía— para evaluar diferencias específicas entre grupos en variables individuales, controlando por covariables cuando fue necesario.

*Con esta base analítica sólida, pasemos a los modelos predictivos.*

---

## SLIDE 7 — MODELOS PREDICTIVOS: RESULTADOS
**⏱ ~4 minutos**

La modelación predictiva tuvo como objetivo responder directamente a la pregunta de investigación: ¿qué tan bien podemos predecir el riesgo de diabetes con distintos conjuntos de variables?

Implementamos dos enfoques: el **QDA** y la **Regresión Logística** en dos versiones.

---

### Modelo 1: QDA — Análisis Discriminante Cuadrático

El QDA es un método de clasificación supervisada que modela la distribución de cada clase de forma separada, asumiendo que cada grupo tiene su propia matriz de covarianza. A diferencia del LDA, no exige que los grupos tengan la misma dispersión —una suposición bastante fuerte que en datos clínicos raramente se cumple.

Lo usamos como **modelo exploratorio** para clasificar individuos en dos categorías: riesgo o no riesgo.

**Resultados del QDA:**
- Exactitud: **76.1%**
- Sensibilidad: **50.6%** — detectó correctamente la mitad de los individuos en riesgo
- Especificidad: **83.5%** — identificó bien a los no diabéticos
- Balanced Accuracy: **67.1%**
- Kappa de Cohen: **0.33** — concordancia moderada

La exactitud del 76% puede parecer aceptable, pero hay que recordar que si el modelo simplemente predijera "no riesgo" para todos, obtendría una exactitud del 76.2% —el porcentaje de la clase mayoritaria. Por eso el Kappa y la balanced accuracy son más informativos aquí.

La sensibilidad del 50.6% es la limitación más crítica desde el punto de vista clínico: el modelo no detecta a uno de cada dos individuos en riesgo. En un contexto de salud pública, eso es un margen de error inaceptable para un instrumento de tamizaje.

Este resultado indicó que se necesitaban variables con mayor poder discriminatorio.

---

### Modelo 2: Regresión Logística — Modelo Básico

El primer modelo logístico incluyó únicamente **variables demográficas y de estilo de vida**: edad, género, IMC, etnicidad, nivel educativo, actividad física y hábito tabáquico. Son variables observables en una consulta inicial, sin necesidad de exámenes de laboratorio.

**Resultados del modelo básico:**
- Exactitud: **73.1%**
- Sensibilidad: **53.4%**
- Especificidad: **66.8%**
- AUC: **0.638**

El AUC de 0.638 indica una capacidad discriminatoria baja-moderada. El modelo es apenas superior a una clasificación aleatoria (AUC = 0.5). Las variables básicas capturan algo de información, pero claramente insuficiente para una predicción confiable.

Los odds ratios del modelo básico confirmaron que la edad, el IMC y la etnicidad tienen asociaciones significativas con el riesgo de diabetes, en la dirección esperada por la literatura. Sin embargo, su poder conjunto es limitado.

---

### Modelo 3: Regresión Logística — Modelo Completo

El modelo completo incorporó, además de las variables básicas, las **variables clínicas**: hemoglobina glicosilada (HbA1c), colesterol total e insulina en sangre. Estas son las variables que en el análisis factorial cargaron en el factor clínico-metabólico —el más directamente ligado al riesgo de diabetes.

**Resultados del modelo completo:**
- Exactitud: **78.5%**
- Sensibilidad: **68.8%** (+15.4 puntos porcentuales sobre el modelo básico)
- Especificidad: **88.2%**
- Balanced Accuracy: **78.5%**
- AUC: **0.871**

El salto más importante es el **AUC: de 0.638 a 0.871**. Esto representa una mejora del 36.5% en la capacidad discriminatoria del modelo. Un AUC de 0.871 se considera bueno en términos clínicos —indica que en el 87.1% de los pares aleatoriamente seleccionados donde uno tiene diabetes y el otro no, el modelo le asigna mayor probabilidad de riesgo al correcto.

La sensibilidad subió de 53.4% a 68.8%, lo que significa que el modelo completo detecta 15 casos de riesgo adicionales por cada 100 que el modelo básico hubiera perdido. En términos de salud pública, esa diferencia es clínicamente significativa.

También clasificó correctamente 246 individuos sin riesgo y 192 individuos en riesgo. La matriz de confusión muestra un desempeño balanceado entre ambas clases.

La curva ROC confirma esta mejora visualmente: el área bajo la curva del modelo completo domina claramente sobre el básico.

---

### ¿Qué variables clínicas son más relevantes?

Los **odds ratios** del modelo completo revelan que la **HbA1c** es el predictor más poderoso: un aumento de una unidad en hemoglobina glicosilada se asocia a un incremento muy significativo en la probabilidad de diabetes, controlando por las demás variables. El colesterol y la insulina también contribuyen de forma significativa, aunque con efectos más moderados.

Esto es consistente con el rol diagnóstico oficial de la HbA1c en las guías clínicas internacionales.

---

### Comparación general

| Métrica            | Modelo Básico | QDA    | Modelo Completo |
|--------------------|--------------|--------|-----------------|
| Exactitud          | 73.1%        | 76.1%  | **78.5%**       |
| Sensibilidad       | 53.4%        | 50.6%  | **68.8%**       |
| Especificidad      | 66.8%        | 83.5%  | **88.2%**       |
| AUC                | 0.638        | ~0.67  | **0.871**       |
| Balanced Accuracy  | 60.1%        | 67.1%  | **78.5%**       |

La conclusión técnica es clara: las variables básicas aportan información inicial, pero **sin las variables clínicas el modelo no alcanza un umbral aceptable de desempeño diagnóstico**.

*Pasemos a las conclusiones finales.*

---

## SLIDE 8 — CONCLUSIONES
**⏱ ~2 minutos**

Tenemos cuatro conclusiones principales.

**Primera — El riesgo de diabetes es un fenómeno genuinamente multivariado.** El MANOVA confirmó que los grupos difieren en perfiles conjuntos. El análisis factorial con rotación oblimin mostró que la variabilidad se organiza en tres dimensiones: dietaria, cardiovascular-demográfica y clínico-metabólica. Estas dimensiones están correlacionadas entre sí, y esa correlación es fisiológicamente real, no un artefacto estadístico.

**Segunda — Las variables básicas son útiles para tamizaje inicial, pero insuficientes para predicción precisa.** El modelo básico logra un AUC de 0.638, lo que es apenas mejor que el azar. Sí captura señales relevantes —edad, IMC, etnicidad— pero no con la precisión necesaria para tomar decisiones clínicas.

**Tercera — Los exámenes clínicos son indispensables.** La HbA1c, el colesterol y la insulina elevan el AUC de 0.638 a 0.871. Son los predictores que marcan la diferencia. Sin ellos, cualquier modelo tiene limitaciones estructurales.

**Cuarta — La estrategia diagnóstica óptima es de dos etapas.** Primero, usar variables básicas para identificar la población de mayor riesgo y orientar la solicitud de exámenes. Segundo, confirmar el perfil con las variables clínicas seleccionadas. Esto permite optimizar recursos sin sacrificar precisión diagnóstica.

---

## SLIDE 9 — CIERRE
**⏱ ~1 minuto**

En resumen: trabajamos con 3.174 individuos, 16 variables y una batería de técnicas estadísticas que van desde el análisis descriptivo hasta la regresión logística. Confirmamos que los grupos de riesgo tienen perfiles distintos, que la estructura latente de los datos se organiza en tres factores correlacionados, y que incorporar variables clínicas —especialmente la HbA1c— es lo que convierte un modelo de tamizaje en un modelo de predicción confiable.

Estos resultados tienen aplicación práctica directa en el diseño de protocolos de detección temprana de diabetes.

Muchas gracias por su atención. Quedamos disponibles para responder preguntas.

---

## POSIBLES PREGUNTAS DEL JURADO

**P: ¿Por qué eligieron rotación oblimin y no varimax?**
> R: La rotación varimax impone ortogonalidad —asume que los factores son independientes. Eso puede ser razonable en algunas áreas, pero en fisiología humana los constructos están naturalmente correlacionados. La dieta afecta el metabolismo, el metabolismo afecta el IMC, el IMC afecta la presión arterial. Forzar independencia habría sido asumir algo que sabemos que es falso. La rotación oblimin permite que los factores se correlacionen, lo que produce una solución más fiel a la biología subyacente. Además, los índices de ajuste del modelo factorial respaldaron esta elección.

**P: ¿Cómo decidieron el número de factores en el análisis factorial?**
> R: Usamos criterios combinados: el criterio de Kaiser —retener factores con valor propio mayor a 1—, el scree plot para identificar el codo de la curva, y el análisis de interpretabilidad de los factores resultantes. Los tres criterios convergieron en tres factores, lo que también tiene coherencia conceptual con los dominios del estudio: dietario, cardiovascular-demográfico y clínico-metabólico.

**P: ¿Por qué restringieron la muestra a mayores de 45 años?**
> R: La prevalencia de diabetes tipo 2 aumenta significativamente a partir de esa edad. Incluir poblaciones más jóvenes habría introducido mucho ruido y diluido los patrones de interés, dado que la enfermedad es mucho menos frecuente en personas jóvenes. Nos enfocamos en la población donde el problema es más prevalente y donde un instrumento de tamizaje tendría mayor utilidad clínica.

**P: ¿Cómo manejaron el desbalance de clases (76% no-diabetes)?**
> R: Fue un factor importante en la evaluación del desempeño. Un modelo que predijera "no riesgo" para todos tendría 76% de exactitud sin aportar nada. Por eso priorizamos el AUC y la balanced accuracy como métricas principales, que son robustas frente al desbalance. El AUC en particular es independiente del umbral de decisión y del balance de clases, lo que lo hace especialmente informativo en este contexto.

**P: ¿Qué diferencia hay entre el QDA y la regresión logística para este problema?**
> R: Son enfoques fundamentalmente distintos. El QDA es un modelo generativo: modela la distribución de cada clase por separado y luego aplica el teorema de Bayes para clasificar. Asume distribución normal multivariada dentro de cada clase, con matrices de covarianza distintas entre grupos. La regresión logística es un modelo discriminativo: modela directamente la probabilidad de pertenencia a una clase como función de las variables predictoras. En la práctica, la regresión logística es más interpretable —los coeficientes se traducen en odds ratios con significado clínico directo— y resultó ser más precisa en este caso, especialmente en sensibilidad.

**P: ¿Qué limitaciones tiene el estudio?**
> R: Identificamos tres principales. Primero, el sesgo de representatividad por la concentración en población hispana limita la generalización. Segundo, los datos son de corte transversal, lo que impide inferir causalidad —sabemos que las variables se asocian al riesgo, pero no podemos concluir que lo causan. Tercero, la base de datos es de 2013-2014; patrones dietarios y de actividad física pueden haber cambiado desde entonces. Como trabajo futuro, sería valioso replicar el análisis con datos más recientes y con muestras más diversas étnica y geográficamente.

**P: ¿El modelo podría implementarse en la práctica clínica?**
> R: El modelo completo —con un AUC de 0.871— tiene un desempeño que ya está en el rango de lo clínicamente útil para un instrumento de tamizaje. Sin embargo, para implementarlo en la práctica se requeriría validación externa con datos independientes, ajuste del umbral de decisión según el contexto clínico (dependiendo de si se prefiere maximizar sensibilidad o especificidad) y consideración de aspectos éticos y regulatorios. Nuestro trabajo establece una base estadística sólida, pero la validación clínica sería el siguiente paso necesario.

---

*Tiempo total estimado: 18–22 minutos (sin preguntas)*
*Preparado por: Samuel S. Arandia Barragán, Juan D. Castañeda Betancourt, Juan F. Rojas Manjarres — Universidad del Rosario, 2026*
