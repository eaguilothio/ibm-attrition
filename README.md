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
Al analizar una problemática organizacional, **observar únicamente la tasa general (16,12%) genera una falsa sensación de normalidad**, ya que se encuentra muy cercana al benchmark del sector tecnológico (15%). 

Sin embargo, el problema general no determinaba un problema uniforme:
* **Invisibilizar problemas específicos al resumir los datos a un solo porcentaje:** Quedarse únicamente con la cifra global oculta que el riesgo está fuertemente concentrado en áreas y condiciones concretas.
* **Diagnóstico y ajuste de recomendaciones:** Para diseñar intervenciones efectivas, es imprescindible analizar las tasas a nivel específico. De lo contrario, se corre el riesgo de aplicar políticas genéricas e ineficaces a toda la plantilla cuando la fuga de talento responde a causas localizadas en grupos reducidos.

--------------------------------------------------------------------------------

#### Resultados
La tasa de rotación global es del 16,12% (237 de 1.470 empleados) — ligeramente por encima del benchmark del sector tecnológico (15%). El dato no es una cifra de alarma en su conjunto, pero esconde una concentración muy clara en perfiles y condiciones específicas:

##### 1. Sales Representative — el rol más vulnerable
**~40% de tasa de rotación** — casi 1 de cada 2 Sales Representatives abandona la empresa.
El rol con mayor fuga de talento del dataset. Un comercial que se va no solo implica el coste de reemplazo — implica pérdida de cartera de clientes y relaciones construidas durante meses. Es el predictor con mayor impacto potencial sobre el negocio.

##### 2. Horas extra — el predictor más claro
**~30% de rotación con OverTime vs ~10% sin OverTime** — los empleados con horas extra tienen tres veces más probabilidad de abandonar.
Es el indicador de desgaste más directo del dataset. Las horas extra sostenidas en el tiempo no son una causa aislada — son la señal de un problema estructural de carga de trabajo o de dimensionamiento de plantilla.

##### 3. Early Tenure (0–2 años) — el periodo crítico
**~30% de rotación en los primeros dos años** — la franja de menor antigüedad concentra la mayor fuga.
Cuando alguien se va en ese periodo, el problema rara vez está en el salario o el plan de carrera. Suele estar en la gestión de expectativas, el onboarding o la falta de acompañamiento inicial — factores que se pueden intervenir antes de que se conviertan en baja.

##### 4. Viajes frecuentes — desgaste por desplazamiento
**~30% con Travel_Frequently vs ~10% con Non-Travel** — viajar frecuentemente triplica la tasa de rotación.
Es el predictor menos visible pero uno de los más potentes. Los desplazamientos frecuentes no suelen aparecer como motivo explícito de baja — pero los datos lo confirman como uno de los factores con mayor correlación con el abandono.

--------------------------------------------------------------------------------

#### Acciones recomendadas
**Representante de ventas — revisión urgente** Analizar si la banda salarial está por debajo del mercado, si la carga de trabajo es sostenible y si las expectativas del rol están bien gestionadas desde la selección. Métrica de seguimiento: tasa de rotación trimestral por rol.

**Horas extra — gestión de la carga** Compensar económicamente las horas extra o revisar el dimensionamiento de plantilla. Las horas extra crónicas son un síntoma, no una solución. Métrica de seguimiento: % de empleados con OverTime activo por departamento.

**Primeros 2 años — programa de acompañamiento** Implementar mentoring estructurado y revisiones de satisfacción periódicas durante los primeros 24 meses. El objetivo es detectar señales de riesgo antes de que se conviertan en baja. Métrica de seguimiento: tasa de rotación en Early Tenure trimestre a trimestre.

**Viajes frecuentes — revisión de política** Analizar qué desplazamientos son realmente necesarios y compensar adecuadamente los que no se pueden eliminar. Métrica de seguimiento: tasa de rotación por categoría de BusinessTravel antes y después de aplicar cambios.

--------------------------------------------------------------------------------

#### Archivos del repositorio
```
.xlsx     → Excel de los datos (hoja datos_crudos)
.pbix     → dashboard Power BI
README.md → documento del proceso
```
