# Informe de Laboratorio: Limpiar, transformar y cargar datos en Power BI (PL-300)

**Autor:** Raúl  
**Curso / Certificación:** PL-300 - Microsoft Power BI Data Analyst  
**Fecha de realización:** 26/10/2026  

---

## 🎯 Objetivo del Laboratorio
En este laboratorio se aplican técnicas de limpieza, transformación e integración de datos utilizando el **Editor de Power Query** en Power BI Desktop para dar forma al modelo semántico de **Adventure Works**, cargando finalmente 7 tablas dimensionadas y de hechos listas para el análisis.

## 📂 Archivos Adjuntos en el Repositorio
* `/starter/01-Starter-Sales Analysis.pbix`: Archivo inicial del laboratorio.
* `/data/ColorFormats.csv` y `/data/ResellerSalesTargets.csv`: Orígenes de datos complementarios.
* `Sales Analysis.pbix`: Archivo final de Power BI con el modelo semántico cargado.

---

## 🛠️ Desarrollo Paso a Paso

### Preparación Inicial
1. Abrir Power BI Desktop y verificar que la sesión esté iniciada (foto de perfil visible en la esquina superior derecha).
2. Abrir el archivo inicial `01-Starter-Sales Analysis.pbix`.
3. Guardar una copia del archivo con el nombre **`Sales Analysis.pbix`**.
4. En la pestaña **Inicio (Home)**, hacer clic en **Transformar datos (Transform data)** para abrir el **Editor de Power Query**.

---

### Ejercicio 1: Configurar la consulta `Salesperson`
En esta tarea se transforma la consulta `DimEmployee` para obtener únicamente a los vendedores y estructurar sus datos de identificación.

1. En el panel **Consultas (Queries)**, seleccionar `DimEmployee` y cambiar su nombre en el panel de Configuración de la consulta a **`Salesperson`**.
2. Localizar la columna **`SalesPersonFlag`** y filtrarla para conservar únicamente los valores **`TRUE`** (vendedores).
3. En la cinta **Inicio**, seleccionar **Elegir columnas (Choose Columns)**, desmarcar todas y conservar únicamente las siguientes 6 columnas:
   * `EmployeeKey`
   * `EmployeeNationalIDAlternateKey`
   * `FirstName`
   * `LastName`
   * `Title`
   * `EmailAddress`
4. Seleccionar simultáneamente las columnas **`FirstName`** y **`LastName`** (manteniendo presionada la tecla `Ctrl`), hacer clic derecho sobre los encabezados y elegir **Combinar columnas (Merge Columns)**:
   * **Separador:** Espacio (*Space*)
   * **Nuevo nombre de columna:** `Salesperson`
5. Renombrar las siguientes columnas:
   * `EmployeeNationalIDAlternateKey` ➔ **`EmployeeID`**
   * `EmailAddress` ➔ **`UPN`**

📸 **Evidencia - Consulta `Salesperson` configurada:**  
![Consulta Salesperson](assets/capturas/01_salesperson.png)

---

### Ejercicio 2: Configurar la consulta `SalespersonRegion`
En esta tarea se prepara la tabla puente que relaciona a los vendedores con las regiones de venta.

1. Seleccionar la consulta `DimEmployeeSalesTerritory` y cambiarle el nombre a **`SalespersonRegion`**.
2. Seleccionar las dos últimas columnas de navegación de tablas (**`DimEmployee`** y **`DimSalesTerritory`**), hacer clic derecho y seleccionar **Quitar columnas (Remove Columns)**, dejando únicamente `EmployeeKey` y `SalesTerritoryKey`.

📸 **Evidencia - Consulta `SalespersonRegion` configurada:**  
![Consulta SalespersonRegion](assets/capturas/02_salesperson_region.png)

---

### Ejercicio 3: Configurar la consulta `Product` e integrar `ColorFormats`
En esta tarea se filtran los productos terminados, se expanden las jerarquías de subcategoría y categoría, y se combina la consulta con un archivo CSV de formatos de color.

#### 3.1. Transformar `DimProduct`
1. Seleccionar la consulta `DimProduct` y renombrarla a **`Product`**.
2. Filtrar la columna **`FinishedGoodsFlag`** seleccionando únicamente **`TRUE`**.
3. Usar **Elegir columnas (Choose Columns)** para conservar solo las siguientes columnas:
   * `ProductKey`
   * `EnglishProductName`
   * `StandardCost`
   * `Color`
   * `DimProductSubcategory`
4. Expandir la columna de registro **`DimProductSubcategory`** (desmarcando *"Usar el nombre de columna original como prefijo"*) y seleccionar únicamente:
   * `EnglishProductSubcategoryName`
   * `DimProductCategory`
5. Expandir la nueva columna **`DimProductCategory`** e incluir únicamente:
   * `EnglishProductCategoryName`
6. Renombrar las siguientes 4 columnas:
   * `EnglishProductName` ➔ **`Product`**
   * `StandardCost` ➔ **`Standard Cost`** *(con espacio)*
   * `EnglishProductSubcategoryName` ➔ **`Subcategory`**
   * `EnglishProductCategoryName` ➔ **`Category`**

#### 3.2. Configurar y combinar `ColorFormats`
1. Seleccionar la consulta **`ColorFormats`**.
2. En la pestaña **Inicio**, hacer clic en **Usar la primera fila como encabezado (Use First Row as Headers)**.
3. Volver a seleccionar la consulta **`Product`**.
4. En la pestaña **Inicio > Combinar**, hacer clic en **Combinar consultas (Merge Queries)**:
   * En la tabla `Product`, seleccionar la columna **`Color`**.
   * En el desplegable inferior, elegir la consulta **`ColorFormats`** y seleccionar su columna **`Color`**.
   * Si aparece la ventana de **Niveles de privacidad (Privacy Levels)**, configurar ambos orígenes como **Organizacional (Organizational)** y guardar.
   * Mantener el tipo de combinación **Externa izquierda (Left Outer)** y hacer clic en **Aceptar**.
5. Expandir la nueva columna **`ColorFormats`** (sin prefijo) seleccionando las dos columnas:
   * `Background Color Format`
   * `Font Color Format`
6. **Deshabilitar la carga de `ColorFormats`:** Seleccionar la consulta `ColorFormats` en el panel izquierdo, hacer clic derecho sobre ella y desmarcar **Habilitar carga (Enable load)** (su nombre pasará a estar en cursiva, ya que sus datos ya fueron integrados en `Product`).

📸 **Evidencia - Consulta `Product` y `ColorFormats` integradas:**  
![Consulta Product](assets/capturas/03_product_colorformats.png)

---

### Ejercicio 4: Configurar la consulta `Reseller`
En esta tarea se limpian los datos de distribuidores, se expande la información geográfica y se corrigen errores tipográficos en los tipos de negocio.

1. Seleccionar la consulta `DimReseller` y renombrarla a **`Reseller`**.
2. Usar **Elegir columnas** para conservar únicamente:
   * `ResellerKey`
   * `BusinessType`
   * `ResellerName`
   * `DimGeography`
3. Expandir la columna **`DimGeography`** e incluir únicamente las siguientes 3 columnas:
   * `City`
   * `StateProvinceName`
   * `EnglishCountryRegionName`
4. Hacer clic derecho sobre el encabezado de **`BusinessType`** y seleccionar **Reemplazar los valores (Replace Values)** para corregir el error de calidad de datos:
   * **Valor que buscar:** `Ware House`
   * **Reemplazar con:** `Warehouse`
5. Renombrar las siguientes 4 columnas:
   * `BusinessType` ➔ **`Business Type`** *(con espacio)*
   * `ResellerName` ➔ **`Reseller`**
   * `StateProvinceName` ➔ **`State-Province`**
   * `EnglishCountryRegionName` ➔ **`Country-Region`**

📸 **Evidencia - Consulta `Reseller` configurada:**  
![Consulta Reseller](assets/capturas/04_reseller.png)

---

### Ejercicio 5: Configurar la consulta `Region`
En esta tarea se depuran los territorios de ventas eliminando la fila correspondiente a la sede central (valor 0).

1. Seleccionar la consulta `DimSalesTerritory` y cambiarle el nombre a **`Region`**.
2. Filtrar la columna **`SalesTerritoryAlternateKey`** desmarcando el valor **`0`** (esto elimina la fila *Corporate HQ*).
3. Usar **Elegir columnas** para conservar únicamente:
   * `SalesTerritoryKey`
   * `SalesTerritoryRegion`
   * `SalesTerritoryCountry`
   * `SalesTerritoryGroup`
4. Renombrar las siguientes 3 columnas:
   * `SalesTerritoryRegion` ➔ **`Region`**
   * `SalesTerritoryCountry` ➔ **`Country`**
   * `SalesTerritoryGroup` ➔ **`Group`**

📸 **Evidencia - Consulta `Region` configurada:**  
![Consulta Region](assets/capturas/05_region.png)

---

### Ejercicio 6: Configurar la consulta `Sales`
En esta tarea se prepara la tabla de hechos de ventas y se corrige el problema de valores nulos en `TotalProductCost` calculando el costo a partir del `StandardCost` del producto.

1. Seleccionar la consulta `FactResellerSales` y renombrarla a **`Sales`**.
2. Usar **Elegir columnas** para conservar únicamente las siguientes 11 columnas:
   * `SalesOrderNumber`
   * `OrderDate`
   * `ProductKey`
   * `ResellerKey`
   * `EmployeeKey`
   * `SalesTerritoryKey`
   * `OrderQuantity`
   * `UnitPrice`
   * `TotalProductCost`
   * `SalesAmount`
   * `DimProduct`
3. Expandir la columna **`DimProduct`** e incluir únicamente la columna **`StandardCost`**.
4. En la pestaña **Agregar columna (Add Column)**, hacer clic en **Columna personalizada (Custom Column)**:
   * **Nuevo nombre de columna:** `Cost`
   * **Fórmula de columna personalizada:**
     ```powerquery
     if [TotalProductCost] = null then [OrderQuantity] * [StandardCost] else [TotalProductCost]
     ```
5. Quitar las columnas **`TotalProductCost`** y **`StandardCost`**.
6. Renombrar las siguientes 3 columnas:
   * `OrderQuantity` ➔ **`Quantity`**
   * `UnitPrice` ➔ **`Unit Price`** *(con espacio)*
   * `SalesAmount` ➔ **`Sales`**
7. Cambiar el tipo de datos de la columna **`Quantity`** a **Número entero (Whole Number)** y verificar que **`Unit Price`**, **`Sales`** y **`Cost`** tengan el tipo de datos **Número decimal fijo (Fixed decimal number / $)**.

📸 **Evidencia - Consulta `Sales` y cálculo de `Cost`:**  
![Consulta Sales](assets/capturas/06_sales.png)

---

### Ejercicio 7: Configurar la consulta `Targets`
En esta tarea se transforma el archivo CSV de objetivos de ventas aplicando anulación de dinamización de columnas (*Unpivot*) para convertir los 12 meses (`M01`-`M12`) en filas y construir una fecha válida.

1. Seleccionar la consulta `ResellerSalesTargets` y renombrarla a **`Targets`**.
2. Seleccionar simultáneamente las columnas **`Year`** y **`EmployeeID`**, hacer clic derecho sobre uno de sus encabezados y seleccionar **Anular dinamización de otras columnas (Unpivot Other Columns)**.
3. En la columna **`Value`**, aplicar un filtro para quitar los valores con guion (**`-`**).
4. Renombrar las dos columnas generadas:
   * `Attribute` ➔ **`MonthNumber`** *(sin espacio)*
   * `Value` ➔ **`Target`**
5. Hacer clic derecho en la columna **`MonthNumber`** y elegir **Reemplazar los valores (Replace Values)**:
   * **Valor que buscar:** `M`
   * **Reemplazar con:** *(dejar en blanco)*
6. Cambiar el tipo de datos de **`MonthNumber`** a **Número entero (Whole Number)**.
7. En la pestaña **Agregar columna (Add Column)**, hacer clic en **Columna a partir de los ejemplos (Column From Examples)**:
   * En la primera fila (correspondiente al año `2017` y mes `7`), escribir **`7/1/2017`** (o `01/07/2017` según la configuración regional de fecha de tu equipo para el 1 de julio de 2017) y presionar `Enter`.
   * Verificar que Power Query combine automáticamente `MonthNumber` y `Year` con el día 1 de cada mes y hacer clic en **Aceptar**.
8. Renombrar la nueva columna creada (`Merged` / `Combinada`) a **`TargetMonth`**.
9. Quitar las columnas **`Year`** y **`MonthNumber`**.
10. Cambiar los tipos de datos de las columnas:
    * **`Target`** ➔ **Número decimal fijo (Fixed decimal number)**
    * **`TargetMonth`** ➔ **Fecha (Date)**
11. Seleccionar la columna **`Target`**, ir a la pestaña **Transformar (Transform) > Columna de número > Estándar (Standard) > Multiplicar (Multiply)**, ingresar **`1000`** y hacer clic en **Aceptar** (ya que los objetivos estaban expresados en miles).

📸 **Evidencia - Consulta `Targets` (Unpivot y Fecha):**  
![Consulta Targets](assets/capturas/07_targets.png)

---

### Ejercicio 8: Revisión Final y Carga al Modelo Semántico
1. Verificar en el panel **Consultas** del Editor de Power Query que existen las **8 consultas** con los nombres exactos:
   * `Salesperson`
   * `SalespersonRegion`
   * `Product`
   * `Reseller`
   * `Region`
   * `Sales`
   * `Targets`
   * *`ColorFormats`* *(en cursiva, carga deshabilitada)*
2. En la pestaña **Inicio**, hacer clic en **Cerrar y aplicar (Close & Apply)**.
3. En la ventana principal de **Power BI Desktop**, verificar en el panel **Datos (Data)** a la derecha que se han cargado las **7 tablas** correctamente en el modelo semántico.
4. Guardar el archivo **`Sales Analysis.pbix`**.

📸 **Evidencia Final - Las 7 tablas cargadas en Power BI Desktop (con foto de perfil y fecha completa):**  
![Modelo Cargado en Power BI Desktop](assets/capturas/08_modelo_final_cargado.png)
