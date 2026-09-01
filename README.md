# People Analytics — IBM
¿Qué factores explican que un empleado abandone la empresa?

> **Resumen:** la rotación global es del 16,12% (237/1.470), pero se concentra en cuatro puntos: **Sales Representative** (~40%), **horas extra** (~30% vs ~10%), **primeros 2 años en la empresa** (~30%) y **viajes frecuentes** (~30% vs ~10%). El detalle y las acciones recomendadas están más abajo.

---

## Índice

1. [¿Cuál era el problema de negocio?](#cuál-era-el-problema-de-negocio)
2. [¿Qué datos usé y de dónde salieron?](#qué-datos-usé-y-de-dónde-salieron)
3. [¿Qué herramienta usé y por qué?](#qué-herramienta-usé-y-por-qué)
4. [Cómo reproducir el dashboard, clic a clic](#cómo-reproducir-el-dashboard-clic-a-clic)
5. [Resultados](#resultados)
6. [Acciones recomendadas](#acciones-recomendadas)
7. [Limitaciones](#limitaciones)
8. [Archivos del repositorio](#archivos-del-repositorio)

---

## ¿Cuál era el problema de negocio?

IBM gestionaba una plantilla de miles de empleados con un problema que los datos de RRHH no terminaban de explicar: por qué ciertos perfiles abandonaban la empresa y otros no.

En este caso práctico, asumí el rol de analista de RRHH utilizando el dataset público IBM HR Analytics para responder una pregunta concreta: **¿Qué factores explican que un empleado abandone la empresa?**

Antes de diseñar cualquier estrategia, hay que saber quién se va y por qué.

---

## ¿Qué datos usé y de dónde salieron?

Dataset público de Kaggle — [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).

**1.470 empleados · 35 variables originales**, cargadas en la hoja `datos_crudos` de `Talento.xlsx`.

De las 35 variables originales se descartaron tres por ser constantes (`EmployeeCount`, `StandardHours`, `Over18`) y el resto por baja relevancia para la pregunta de negocio.

Variables seleccionadas:

| Dimensión | Variables |
|---|---|
| Objetivo | `Attrition` |
| Posición en la empresa | `Department` `JobRole` |
| Condiciones del puesto | `OverTime` `BusinessTravel` |
| Trayectoria | `YearsAtCompany` |
| Calculada | `Tenure Bracket` |

---

## ¿Qué herramienta usé y por qué?

Usé Power BI.

- Es la herramienta de visualización y análisis más extendida en el entorno empresarial, especialmente en contextos de RRHH y reporting.
- Permite construir dashboards interactivos —a diferencia de Excel o Python, que ofrecen gráficos estáticos— con filtros que se actualizan en tiempo real.
- El foco del proyecto es traducir los datos en insights visuales — Power BI es la herramienta idónea para ese objetivo.

---

## Cómo reproducir el dashboard, clic a clic

Esta sección documenta el proceso exacto seguido en Power BI Desktop, para que el proyecto sea reproducible por cualquier persona que abra el repositorio sin haber visto el `.pbix` antes.

### Paso 1 — Carga y selección de variables

1. Abrir **Power BI Desktop** → pestaña **Inicio** → **Obtener datos** → **Excel**.
2. Seleccionar `Talento.xlsx` → en el navegador, marcar la hoja **`datos_crudos`** → clic en **Transformar datos** (nunca "Cargar" directo: así se revisa y limpia antes de que los datos entren al modelo).
3. Dentro del **Editor de Poweer Query**, con todas las columnas visibles, seleccionar con `Ctrl + clic` las 6 columnas que sí se van a usar: `Attrition`, `Department`, `JobRole`, `OverTime`, `BusinessTravel`, `YearsAtCompany`.
4. Clic derecho sobre cualquiera de los encabezados seleccionados → **Quitar otras columnas**. Esto elimina de una vez las 29 columnas no seleccionadas (incluidas las 3 constantes `EmployeeCount`, `StandardHours`, `Over18`), en lugar de borrarlas una a una.
5. Revisar en el panel derecho **Configuración de la consulta → Pasos aplicados** que solo queden dos pasos: `Origen` y `Columnas quitadas`. Esto mantiene la transformación auditable.
6. **Inicio → Cerrar y aplicar.**

### Paso 2 — Columna calculada: `Tenure Bracket`

Los años de antigüedad por sí solos no tienen significado operativo en RRHH, así que se segmentan en tramos del ciclo de vida del empleado.

1. Cambiar a la **Vista de informe** (icono de gráfico de barras en el panel izquierdo).
2. En el panel **Datos** (derecha), clic derecho sobre la tabla `datos_crudos` → **Nueva columna**.
3. Escribir la fórmula:
   ```dax
   Tenure Bracket = 
   SWITCH(
       TRUE(),
       datos_crudos[YearsAtCompany] <= 2, "Early",
       datos_crudos[YearsAtCompany] <= 5, "Growing",
       datos_crudos[YearsAtCompany] <= 10, "Established",
       "Veteran"
   )
   ```
4. **Enter** para confirmar. `YearsAtCompany` se mantiene en el modelo (no se oculta) porque `Tenure Bracket` depende de ella.

### Paso 3 — Medidas DAX, en una tabla dedicada

A diferencia de una columna calculada, una medida se recalcula dinámicamente según los filtros y segmentaciones activas en cada momento. Para no mezclarlas con las columnas de datos, las tres medidas viven en una tabla propia llamada `_Medidas` (buena práctica estándar de modelado en Power BI, especialmente útil cuando el número de medidas crece).

1. **Inicio → Introducir datos.** Dejar una sola columna con un único valor (por ejemplo `Column` = 1) y en el campo **Nombre** escribir `_Medidas`. Clic en **Cargar**. Esta tabla no se conecta con nada del modelo — es solo un contenedor.
2. Con la tabla `_Medidas` seleccionada en el panel **Datos**, clic derecho → **Nueva medida** (se repite tres veces, una por medida).
3. **Medida 1 — Total Empleados:**
   ```dax
   Total Empleados = COUNTROWS(datos_crudos)
   ```
4. **Medida 2 — Empleados Rotados:**
   ```dax
   Empleados Rotados = CALCULATE(COUNTROWS(datos_crudos), datos_crudos[Attrition] = "Yes")
   ```
5. **Medida 3 — Tasa de Rotación:**
   ```dax
   Tasa de Rotación = DIVIDE([Empleados Rotados], [Total Empleados], 0)
   ```
6. Con `Tasa de Rotación` seleccionada en el panel Datos, ir a la pestaña **Herramientas de medida → Formato → Porcentaje**, 2 decimales.

### Paso 4 — Construcción del dashboard

El lienzo se organiza en tres franjas: KPIs arriba, segmentaciones a la izquierda, gráficos de barras en el centro.

**KPIs (fila superior):**

1. **Insertar → Elementos visuales → Tarjeta**. Repetir 3 veces.
2. Arrastrar al campo **Datos del campo** de cada tarjeta, una medida distinta: `Total Empleados`, `Empleados Rotados`, `Tasa de Rotación`.
3. Posicionar de izquierda a derecha en ese orden en la parte superior del lienzo (y = 0).

**Segmentaciones (columna izquierda):**

4. **Insertar → Elementos visuales → Segmentación**. Repetir 3 veces.
5. Arrastrar `Department` al campo de la primera, `JobRole` a la segunda, `OverTime` a la tercera.
6. Colocarlas apiladas en la columna izquierda del lienzo (`Department` arriba, `JobRole` debajo; `OverTime` se coloca más pequeña, junto al gráfico de OverTime).

**Gráficos de barras (cuerpo central, 5 en total):**

7. **Insertar → Elementos visuales → Gráfico de columnas agrupadas**. Repetir 5 veces, uno por dimensión.
8. En cada uno: arrastrar la dimensión al **Eje** y `Tasa de Rotación` a **Valores**:
   - `Department` → Tasa de Rotación
   - `JobRole` → Tasa de Rotación
   - `OverTime` → Tasa de Rotación
   - `Tenure Bracket` → Tasa de Rotación
   - `BusinessTravel` → Tasa de Rotación
9. **Formato → Eje X → Ordenar por** el valor de la medida, de mayor a menor, para que el problema salte a la vista sin tener que leer etiquetas.

**Interacciones y formato final:**

10. Verificar interacciones cruzadas: seleccionar cualquier gráfico → pestaña **Formato → Editar interacciones**. Confirmar que las 3 segmentaciones filtran los 5 gráficos y las 3 tarjetas simultáneamente (comportamiento por defecto de Power BI, no requiere configuración adicional salvo excepciones puntuales).
11. **Vista → Temas** → aplicar un tema accesible con buen contraste para los colores de barras.
12. **Formato de página → Organizar → Alinear** los visuales para dejar la cuadrícula limpia: tarjetas alineadas por su borde superior, gráficos por su borde inferior.

---

## Resultados

La tasa de rotación global es del 16,12% (237 de 1.470 empleados) — ligeramente por encima del benchmark del sector tecnológico (15%). El dato no es una cifra de alarma, pero esconde una concentración muy clara: el problema no está repartido uniformemente, está localizado en perfiles y condiciones específicas.

### 1. Sales Representative — el rol más vulnerable

**~40% de tasa de rotación** — casi 1 de cada 2 Sales Representatives abandona la empresa.

El rol con mayor fuga de talento del dataset. Un comercial que se va no solo implica el coste de reemplazo — implica pérdida de cartera de clientes y relaciones construidas durante meses. Es el predictor con mayor impacto potencial sobre el negocio.

### 2. Horas extra — el predictor más claro

**~30% de rotación con OverTime vs ~10% sin OverTime** — los empleados con horas extra tienen tres veces más probabilidad de abandonar.

Es el indicador de desgaste más directo del dataset. Las horas extra sostenidas en el tiempo no son una causa aislada — son la señal de un problema estructural de carga de trabajo o de dimensionamiento de plantilla.

### 3. Early Tenure (0–2 años) — el periodo crítico

**~30% de rotación en los primeros dos años** — la franja de menor antigüedad concentra la mayor fuga.

Cuando alguien se va en ese periodo, el problema rara vez está en el salario o el plan de carrera. Suele estar en la gestión de expectativas, el onboarding o la falta de acompañamiento inicial — factores que se pueden intervenir antes de que se conviertan en baja.

### 4. Viajes frecuentes — desgaste por desplazamiento

**~30% con Travel_Frequently vs ~10% con Non-Travel** — viajar frecuentemente triplica la tasa de rotación.

Es el predictor menos visible pero uno de los más potentes. Los desplazamientos frecuentes no suelen aparecer como motivo explícito de baja — pero los datos lo confirman como uno de los factores con mayor correlación con el abandono.

---

## Acciones recomendadas

**Representante de ventas — revisión urgente**
Analizar si la banda salarial está por debajo del mercado, si la carga de trabajo es sostenible y si las expectativas del rol están bien gestionadas desde la selección. Métrica de seguimiento: tasa de rotación trimestral por rol.

**Horas extra — gestión de la carga**
Compensar económicamente las horas extra o revisar el dimensionamiento de plantilla. Las horas extra crónicas son un síntoma, no una solución. Métrica de seguimiento: % de empleados con OverTime activo por departamento.

**Primeros 2 años — programa de acompañamiento**
Implementar mentoring estructurado y revisiones de satisfacción periódicas durante los primeros 24 meses. El objetivo es detectar señales de riesgo antes de que se conviertan en baja. Métrica de seguimiento: tasa de rotación en Early Tenure trimestre a trimestre.

**Viajes frecuentes — revisión de política**
Analizar qué desplazamientos son realmente necesarios y compensar adecuadamente los que no se pueden eliminar. Métrica de seguimiento: tasa de rotación por categoría de BusinessTravel antes y después de aplicar cambios.

---

## Archivos del repositorio

```
.xlsx     → Excel de los datos (hoja datos_crudos)
.pbix     → dashboard Power BI
README.md → documento del proceso
```
