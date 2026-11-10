# Data Cleansing, Analytics & Customer Segmentation 📊📱

Este repositorio contiene un proyecto final de análisis y limpieza de datos enfocado en el sector de telecomunicaciones. El objetivo principal fue auditar la calidad de la información de tres bases de datos relacionales (`users`, `usage` y un histórico de planes), corregir anomalías estructurales y diseñar una segmentación lógica de clientes para la toma de decisiones comerciales.

El desarrollo se realizó íntegramente en **Python** utilizando **Jupyter Notebooks**.

## 🚀 Características del Proyecto

* **Auditoría de Calidad:** Cálculo exhaustivo de proporciones de valores nulos y detección de datos atípicos.
* **Limpieza y Corrección de Datos:** Identificación e imputación de valores centinela numéricos y de texto, así como el tratamiento de inconsistencias temporales (fechas futuras).
* **Análisis de Mecanismos Faltantes:** Demostración estadística de nulos del tipo **MAR** (*Missing At Random*) mediante correlación cruzada con variables de negocio.
* **Ingeniería de Características (Feature Engineering):** Agregación masiva y condicional de métricas de consumo por usuario a través de pipelines de Pandas.
* **Análisis Exploratorio de Datos (EDA):** Visualización avanzada empleando histogramas superpuestos con líneas de tendencia (KDE) y diagramas de caja (Boxplots) estructurados mediante bucles automatizados.
* **Segmentación Multivariable:** Construcción de perfiles de uso y rangos de edad aplicando lógica condicional vectorizada.

## 🛠️ Tecnologías y Librerías Utilizadas

* **Python 3.x**
* **Pandas:** Para la manipulación, agregación, cruce (`merge`) y limpieza de estructuras de datos.
* **NumPy:** Para el soporte matemático y manejo de vectores nulos oficiales (`np.nan`).
* **Seaborn:** Para la creación de gráficos estadísticos avanzados (`histplot`, `boxplot`, `countplot`).
* **Matplotlib.pyplot:** Para la personalización de entornos gráficos, títulos y exportación de imágenes.

## 📈 Hallazgos y Análisis Ejecutivo

### ⚠️ Calidad de Datos y Soluciones Aplicadas
1. **Centinelas Numéricos (`age`):** Se identificó el uso de `-999` para representar registros omitidos. Se solucionó mediante la imputación de la **mediana real** (calculada estrictamente con edades > 0), restaurando la desviación estándar a un rango humano lógico.
2. **Centinelas de Texto (`city`):** Se aislaron 96 registros guardados con el carácter `?`. Fueron estandarizados al formato nulo oficial de Python (`NaN`).
3. **Anomalías Temporales (`reg_date`):** Se detectó un 1% de registros corruptos apuntando al año **2026** (fechas futuras incoherentes con el contexto). Se marcaron formalmente como nulos temporales (`NaT`).
4. **Nulos Justificados (MAR):** Se comprobó que el 55.19% de nulos en `duration` y el 44.74% en `length` dependían enteramente de la columna `type` (las llamadas no generan caracteres de texto y los SMS no acumulan minutos). Los datos estaban íntegros y se mantuvieron para el análisis por segmentos.

### 🔍 Insights de Segmentación y Negocio
* **Segmento por Edad:** Los usuarios presentan una **distribución uniforme** (rectangular). No existe correlación alguna entre la edad y el tipo de suscripción; los jóvenes, adultos y adultos mayores contratan los planes Básico y Premium en proporciones idénticas.
* **Segmento por Nivel de Uso:** Se descubrió una polarización drástica de la base instalada. La gran mayoría de la población pertenece a **Bajo Uso**, mientras que un bloque selecto pertenece a **Alto Uso**. El segmento intermedio reportó cero registros debido a la rigidez del operador lógico `AND` originalmente propuesto, lo que abre una ventana para optimizar las reglas de segmentación de la empresa.
* **Comportamiento de Planes:** Los patrones de uso demuestran que los usuarios del plan Premium consumen un volumen promedio de llamadas y mensajes idéntico al del plan Básico. El valor del plan Premium reside actualmente en su tarifa fija de suscripción y no en una demanda técnica mayor por parte del cliente.

### 🎯 Gestión de Outliers (Usuarios Extremos)
Mediante el método matemático del **Rango Intercuartílico (IQR)** se aislaron de forma precisa los usuarios con consumos excepcionalmente altos (*Heavy Users*):
* **Mensajes:** 46 usuarios superaron el límite de 11.50 mensajes.
* **Llamadas:** 30 usuarios superaron el límite de 10.50 llamadas.
* **Minutos de llamadas:** 109 usuarios excedieron el límite de 61.86 minutos.

## 💡 Recomendaciones Estratégicas Presentadas
* **Rediseño de Oferta de SMS:** Empaquetar los mensajes de texto como un beneficio "ilimitado" en ambos planes. Dado que el grueso del mercado consume apenas entre 4 y 7 mensajes mensuales, esta acción elevará drásticamente la percepción de valor de la marca a costo cero de infraestructura.
* **Monitoreo Automático de Heavy Users:** Implementar alertas de negocio utilizando los límites superiores de outliers calculados (ej. clientes con más de 61 minutos consumidos) para ofrecerles proactivamente planes de datos dedicados o tarifas empresariales antes de que incurran en abandono de servicio (*churn*).
* **Flexibilización de Reportes:** Reemplazar las condiciones estrictas `AND` en los scripts de segmentación por operadores `OR` o sistemas de puntaje acumulado para capturar correctamente a los consumidores híbridos en la categoría de "Uso Medio".
