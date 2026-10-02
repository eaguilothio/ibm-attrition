# Employee Attrition Analysis — IBM
¿Qué factores explican que un empleado abandone la empresa?

**Resumen:** la rotación global es del 16,12% (237/1.470), pero se concentra en cuatro puntos: **Sales Representative** (~40%), **horas extra** (~30% vs ~10%), **primeros 2 años en la empresa** (~30%) y **viajes frecuentes** (~30% vs ~10%). Además, el análisis destaca un hallazgo metodológico clave: resumir los datos a un solo porcentaje invisibiliza problemas específicos que requieren atención.

--------------------------------------------------------------------------------

#### Índice
1. [¿Cuál era el problema de negocio?](#cuál-era-el-problema-de-negocio)
2. [Hallazgo metodológico clave: Tasa general vs. específicas](#hallazgo-metodológico-clave-tasa-general-vs-específicas)
3. [Resultados](#resultados)
4. [Acciones recomendadas](#acciones-recomendadas)
5. [Archivos del repositorio](#archivos-del-repositorio)

--------------------------------------------------------------------------------

#### ¿Cuál era el problema de negocio?
IBM gestionaba una plantilla de miles de empleados con un problema que los datos de RRHH no terminaban de explicar: por qué ciertos perfiles abandonaban la empresa y otros no.
En este caso práctico, asumí el rol de analista de RRHH utilizando el dataset público IBM HR Analytics para responder una pregunta concreta: **¿Qué factores explican que un empleado abandone la empresa?**
Antes de diseñar cualquier estrategia, hay que saber quién se va y por qué.

--------------------------------------------------------------------------------

#### Hallazgo metodológico clave: Tasa general vs. específicas

Al analizar la problemática de abandono, **observamos una tasa general de rotación del 16,12%**. A simple vista, este porcentaje puede dar una **falsa sensación de normalidad**, ya que no parece una cifra especialmente alarmante.

Sin embargo, al profundizar en los datos, vemos que **la rotación no se distribuye de forma uniforme**. La tasa general puede ocultar diferencias importantes entre determinados grupos.

* **Invisibilizar problemas específicos:** quedarse únicamente con el porcentaje global hace que pasen desapercibidos los colectivos donde la rotación es mucho más elevada.

* **Diagnóstico y recomendaciones:** para entender realmente dónde está el problema, es necesario analizar las tasas de rotación por diferentes segmentos. De lo contrario, podríamos acabar aplicando recomendaciones generales a toda la plantilla cuando el problema está más concentrado en determinados grupos.


--------------------------------------------------------------------------------

#### Resultados
La tasa de rotación global es del 16,12% (237 de 1.470 empleados) — ligeramente por encima del benchmark del sector tecnológico (15%). El dato no es una cifra de alarma en su conjunto, pero esconde una concentración muy clara en perfiles y condiciones específicas:

##### 1. Sales Representative
**~40% de tasa de rotación** — casi 1 de cada 2 Sales Representatives abandona la empresa.
Es el rol con la mayor tasa de rotación del dataset. Una rotación elevada en este grupo puede tener implicaciones operativas relevantes, especialmente en puestos donde la experiencia y las relaciones con clientes son importantes.

##### 2. Horas extra 
**~30% de rotación con OverTime vs ~10% sin OverTime** — casi 3× más rotación entre quienes hacen horas extras.
La asociación entre OverTime y rotación apunta a una posible relación entre carga de trabajo y abandono. Pueden ser la señal de un problema estructural de carga de trabajo o de dimensionamiento de plantilla.

##### 3. Early Tenure (0–2 años)
**~30% de rotación en los primeros dos años** — la franja de menor antigüedad concentra la mayor fuga.
Cuando alguien se va en ese periodo, puede estar relacionado con la gestión de expectativas o la falta de acompañamiento inicial — factores que se pueden intervenir antes de que se conviertan en baja.

##### 4. Viajes frecuentes — desgaste por desplazamiento
**~30% con Travel_Frequently vs ~10% con Non-Travel** — viajar frecuentemente triplica la tasa de rotación.
Los desplazamientos frecuentes puede ser un factor relevante a considerar en el abandono.

--------------------------------------------------------------------------------

#### Acciones recomendadas
**Representante de ventas — programa de acompañamiento** 
Analizar si la banda salarial está alineada con el mercado, si la carga de trabajo es sostenible y si las expectativas del rol están bien gestionadas. Implementar un programa de acompañamiento para detectar posibles señales de insatisfacción y riesgo de abandono.

**Horas extra — gestión de la carga** 
Analizar la frecuencia y duración de las horas extra por equipo y puesto. Revisar el dimensionamiento de plantilla y evaluar medidas de compensación cuando las horas extra sean recurrentes.

**Primeros 2 años — programa de acompañamiento** 
Implementar acompañamiento y revisiones periódicas de satisfacción durante los primeros 24 meses. El objetivo es detectar tempranamente posibles problemas de integración y adaptación al puesto.

**Viajes frecuentes — revisión de viajes** 
Analizar qué viajes de negocio son realmente necesarios y establecer alternativas cuando sea posible. Revisar también las condiciones de compensación de los desplazamientos que no puedan eliminarse.

Revisar periódicamente la evolución de la rotación asociada a estos factores de riesgo.

--------------------------------------------------------------------------------

#### Archivos del repositorio
```
.xlsx     → Excel de los datos (hoja datos_crudos)
.pbix     → dashboard Power BI
README.md → documento del proceso
```
