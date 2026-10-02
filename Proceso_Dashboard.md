# Proceso Técnico y Metodología — IBM Employee Attrition

#### Índice
1. [¿Qué datos usé y de dónde salieron?](#qué-datos-usé-y-de-dónde-salieron)
2. [¿Qué herramienta usé y por qué?](#qué-herramienta-usé-y-por-qué)
3. [Estrategia analítica y enfoque de diseño](#estrategia-analítica-y-enfoque-de-diseño)
4. [Cómo reproducir el dashboard, clic a clic](#cómo-reproducir-el-dashboard-clic-a-clic)

--------------------------------------------------------------------------------

#### ¿Qué datos usé y de dónde salieron?
Dataset público de Kaggle — [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).
**1.470 empleados · 35 variables originales**, cargadas en la hoja datos_crudos de Talento.xlsx.
De las 35 variables originales se descartaron tres por ser constantes (EmployeeCount, StandardHours, Over18) y el resto por baja relevancia para la pregunta de negocio.
Variables seleccionadas:
| Dimensión | Variables |
| ------ | ------ |
| Objetivo | Attrition |
| Posición en la empresa | Department JobRole |
| Condiciones del puesto | OverTime BusinessTravel |
| Trayectoria | YearsAtCompany |
| Calculada | Tenure Bracket |

--------------------------------------------------------------------------------

#### ¿Qué herramienta usé y por qué?
Usé Power BI.
* Es la herramienta de visualización y análisis más extendida en el entorno empresarial, especialmente en contextos de RRHH y reporting.
* Permite construir dashboards interactivos —a diferencia de Excel o Python, que ofrecen gráficos estáticos— con filtros que se actualizan en tiempo real.
* El foco del proyecto es traducir los datos en insights visuales — Power BI es la herramienta idónea para ese objetivo.

--------------------------------------------------------------------------------

#### Estrategia analítica y enfoque de diseño
El diseño del modelo de datos y del lienzo responde directamente a una premisa metodológica: **evitar invisibilizar problemas específicos al resumir los datos a un solo porcentaje**.

* **Superación del indicador único:** Si el dashboard solo mostrase la tarjeta global de tasa de rotación (16,12%), se transmitiría una falsa sensación de normalidad por su cercanía al benchmark del sector (15%).
* **Desglose visual por dimensiones clave:** Por ello, el lienzo no se limita a métricas agregadas, sino que distribuye 5 gráficos de barras (JobRole, OverTime, Tenure Bracket, BusinessTravel y Department) ordenados de mayor a menor rotación. Esto obliga al usuario a detectar de inmediato dónde se concentran las tasas críticas (hasta el 40%).
* **Interactividad y cálculo dinámico:** El uso de medidas DAX dinámicas combinadas con segmentadores cruzados permite aislar variables y entender el comportamiento específico de cada subgrupo sin perder la referencia global.

--------------------------------------------------------------------------------

#### Cómo reproducir el dashboard, clic a clic
Esta sección documenta el proceso exacto seguido en Power BI Desktop, para que el proyecto sea reproducible por cualquier persona que abra el repositorio sin haber visto el .pbix antes.

##### Paso 1 — Carga y selección de variables
1. Abrir **Power BI Desktop** → pestaña **Inicio** → **Obtener datos** → **Excel**.
2. Seleccionar Talento.xlsx → en el navegador, marcar la hoja **datos_crudos** → clic en **Transformar datos** (nunca "Cargar" directo: así se revisa y limpia antes de que los datos entren al modelo).
3. Dentro del **Editor de Power Query**, con todas las columnas visibles, seleccionar con Ctrl + clic las 6 columnas que sí se van a usar: Attrition, Department, JobRole, OverTime, BusinessTravel, YearsAtCompany.
4. Clic derecho sobre cualquiera de los encabezados seleccionados → **Quitar otras columnas**. Esto elimina de una vez las 29 columnas no seleccionadas (incluidas las 3 constantes EmployeeCount, StandardHours, Over18), en lugar de borrarlas una a una.
5. Revisar en el panel derecho **Configuración de la consulta → Pasos aplicados** que solo queden dos pasos: Origen y Columnas quitadas. Esto mantiene la transformación auditable.
6. **Inicio → Cerrar y aplicar.**

##### Paso 2 — Columna calculada: Tenure Bracket
Los años de antigüedad por sí solos no tienen significado operativo en RRHH, así que se segmentan en tramos del ciclo de vida del empleado.
1. Cambiar a la **Vista de informe** (icono de gráfico de barras en el panel izquierdo).
2. En el panel **Datos** (derecha), clic derecho sobre la tabla datos_crudos → **Nueva columna**.
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
4. **Enter** para confirmar. YearsAtCompany se mantiene en el modelo (no se oculta) porque Tenure Bracket depende de ella.

##### Paso 3 — Medidas DAX, en una tabla dedicada
A diferencia de una columna calculada, una medida se recalcula dinámicamente según los filtros y segmentaciones activas en cada momento. Para no mezclarlas con las columnas de datos, las tres medidas viven en una tabla propia llamada _Medidas (buena práctica estándar de modelado en Power BI, especialmente útil cuando el número de medidas crece).
1. **Inicio → Introducir datos.** Dejar una sola columna con un único valor (por ejemplo Column = 1) y en el campo **Nombre** escribir _Medidas. Clic en **Cargar**. Esta tabla no se conecta con nada del modelo — es solo un contenedor.
2. Con la tabla _Medidas seleccionada en el panel **Datos**, clic derecho → **Nueva medida** (se repite tres veces, una por medida).
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
6. Con Tasa de Rotación seleccionada en el panel Datos, ir a la pestaña **Herramientas de medida → Formato → Porcentaje**, 2 decimales.

##### Paso 4 — Construcción del dashboard
El lienzo se organiza en tres franjas: KPIs arriba, segmentaciones a la izquierda, gráficos de barras en el centro.

**KPIs (fila superior):**
1. **Insertar → Elementos visuales → Tarjeta**. Repetir 3 veces.
2. Arrastrar al campo **Datos del campo** de cada tarjeta, una medida distinta: Total Empleados, Empleados Rotados, Tasa de Rotación.
3. Posicionar de izquierda a derecha en ese orden en la parte superior del lienzo (y = 0).

**Segmentaciones (columna izquierda):**
4. **Insertar → Elementos visuales → Segmentación**. Repetir 3 veces.
5. Arrastrar Department al campo de la primera, JobRole a la segunda, OverTime a la tercera.
6. Colocarlas apiladas en la columna izquierda del lienzo (Department arriba, JobRole debajo; OverTime se coloca más pequeña, junto al gráfico de OverTime).

**Gráficos de barras (cuerpo central, 5 en total):**
7. **Insertar → Elementos visuales → Gráfico de columnas agrupadas**. Repetir 5 veces, uno por dimensión.
8. En cada uno: arrastrar la dimensión al **Eje** y Tasa de Rotación a **Valores**:
    * Department → Tasa de Rotación
    * JobRole → Tasa de Rotación
    * OverTime → Tasa de Rotación
    * Tenure Bracket → Tasa de Rotación
    * BusinessTravel → Tasa de Rotación
9. **Formato → Eje X → Ordenar por** el valor de la medida, de mayor a menor, para que el problema salte a la vista sin tener que leer etiquetas.

**Interacciones y formato final:**
10. Verificar interacciones cruzadas: seleccionar cualquier gráfico → pestaña **Formato → Editar interacciones**. Confirmar que las 3 segmentaciones filtran los 5 gráficos y las 3 tarjetas simultáneamente (comportamiento por defecto de Power BI, no requiere configuración adicional salvo excepciones puntuales).
11. **Vista → Temas** → aplicar un tema accesible con buen contraste para los colores de barras.
12. **Formato de página → Organizar → Alinear** los visuales para dejar la cuadrícula limpia: tarjetas alineadas por su borde superior, gráficos por su borde inferior.
