# 📊 Informe de Laboratorio: Clean, Transform, and Load Data in Power BI (PL-300)

* **Autor:** Raúl (`raulchuco2000`)
* **Curso / Certificación:** PL-300 - Microsoft Power BI Data Analyst
* **Práctica:** Limpiar, transformar y cargar datos en Power BI (Adventure Works)
* **Fecha:** 26/10/2026

---

## 👤 Verificación de Perfil de Power BI y Fecha Completa

![Verificación de Perfil y Fecha en Power BI](<img width="1668" height="972" alt="Captura de pantalla_PowerBI" src="https://github.com/user-attachments/assets/508d5664-475e-461d-b32a-fc6e7aab71ee" />
)

---

## 🎯 Resumen Ejecutivo del Laboratorio
En este laboratorio se llevó a cabo el proceso completo de extracción, limpieza, transformación y carga (ETL) en **Power Query** dentro de **Power BI Desktop** a partir de la base de datos dimensional `AdventureWorksDW2020` y dos orígenes de datos complementarios en formato CSV (`ColorFormats.csv` y `ResellerSalesTargets.csv`). El modelo semántico resultante consta de **7 tablas** optimizadas para el análisis comercial.

---

## 🛠️ Desarrollo Paso a Paso y Tablas Resultantes

### Ejercicio 1: Configuración de la consulta `Salesperson`
Se partió de la tabla `DimEmployee` para aislar únicamente a los empleados que pertenecen al departamento de ventas:
1. Se renombró la consulta `DimEmployee` a **`Salesperson`**.
2. Se filtró la columna **`SalesPersonFlag = TRUE`** (obteniendo 18 vendedores activos).
3. Se seleccionaron las columnas `EmployeeKey`, `EmployeeNationalIDAlternateKey`, `FirstName`, `LastName`, `Title` y `EmailAddress`.
4. Se combinaron las columnas **`FirstName`** y **`LastName`** separadas por un espacio para crear la columna **`Salesperson`**.
5. Se renombró `EmployeeNationalIDAlternateKey` a **`EmployeeID`** y `EmailAddress` a **`UPN`**.

#### 📊 Tabla Resultante: `Salesperson` (18 filas)
| EmployeeKey | EmployeeID | Salesperson | Title | UPN |
| :---: | :---: | :--- | :--- | :--- |
| 272 | 502097814 | Stephen Jiang | North America Sales Manager | stephen0@adventure-works.com |
| 277 | 112432117 | Brian Welcker | Director of Sales | brian3@adventure-works.com |
| 281 | 841560125 | Michael Blythe | Sales Representative | michael9@adventure-works.com |
| 282 | 191644724 | Linda Mitchell | Sales Representative | linda3@adventure-works.com |
| 283 | 615389812 | Jillian Carson | Sales Representative | jillian0@adventure-works.com |
| 284 | 234474252 | Garrett Vargas | Sales Representative | garrett1@adventure-works.com |
| 285 | 716374314 | Tsvi Reiter | Sales Representative | tsvi0@adventure-works.com |
| 286 | 758596752 | Pamela Ansman-Wolfe | Sales Representative | pamela0@adventure-works.com |
| 287 | 982310417 | Shu Ito | Sales Representative | shu0@adventure-works.com |
| 288 | 954276278 | José Saraiva | Sales Representative | jose1@adventure-works.com |
| 289 | 668991357 | David Campbell | Sales Representative | david8@adventure-works.com |
| 290 | 134219713 | Amy Alberts | European Sales Manager | amy0@adventure-works.com |
| 291 | 295847284 | Jae Pak | Sales Representative | jae0@adventure-works.com |
| 292 | 709100352 | Ranjit Varkey Chudukatil | Sales Representative | ranjit0@adventure-works.com |
| 293 | 908361952 | Tete Mensa-Annan | Sales Representative | tete0@adventure-works.com |
| 294 | 481044938 | Syed Abbas | Pacific Sales Manager | syed0@adventure-works.com |
| 295 | 953083303 | Rachel Valdez | Sales Representative | rachel0@adventure-works.com |
| 296 | 566849003 | Lynn Tsoflias | Sales Representative | lynn0@adventure-works.com |

---

### Ejercicio 2: Configuración de la consulta `SalespersonRegion`
Se configuró la tabla puente entre vendedores y territorios de venta:
1. Se renombró la consulta `DimEmployeeSalesTerritory` a **`SalespersonRegion`**.
2. Se quitaron las columnas de navegación `DimEmployee` y `DimSalesTerritory`, conservando las claves **`EmployeeKey`** y **`SalesTerritoryKey`** (39 filas).

#### 📊 Tabla Resultante: `SalespersonRegion`
| EmployeeKey | SalesTerritoryKey |
| :---: | :---: |
| 272 | 1 |
| 272 | 2 |
| 272 | 3 |
| 281 | 2 |
| 281 | 3 |
| 281 | 5 |
| 282 | 3 |
| 282 | 4 |
| 283 | 3 |
| 284 | 6 |
| 285 | 5 |
| 286 | 1 |
| 286 | 4 |
| 287 | 4 |
| 288 | 6 |
| 289 | 1 |
| 290 | 7 |
| 290 | 8 |
| 291 | 7 |
| 291 | 10 |
| 292 | 7 |
| 293 | 1 |
| 294 | 9 |
| 295 | 8 |
| 296 | 9 |

---

### Ejercicio 3: Configuración de la consulta `Product` e integración de `ColorFormats`
1. Se renombró `DimProduct` a **`Product`** y se filtró **`FinishedGoodsFlag = TRUE`** (397 productos terminados).
2. Se conservaron las columnas `ProductKey`, `EnglishProductName`, `StandardCost`, `Color` y `DimProductSubcategory`.
3. Se expandió `DimProductSubcategory` para obtener `EnglishProductSubcategoryName` y `DimProductCategory`, y luego se expandió `DimProductCategory` para incluir `EnglishProductCategoryName`.
4. Se renombraron las columnas a **`Product`**, **`Standard Cost`**, **`Subcategory`** y **`Category`**.
5. En la consulta **`ColorFormats`**, se promovió la primera fila como encabezados (`Use First Row as Headers`).
6. Se realizó un **Merge Queries (Left Outer Join)** entre `Product` y `ColorFormats` mediante la columna **`Color`**, expandiendo **`Background Color Format`** y **`Font Color Format`**.
7. Se deshabilitó la carga (**`Enable load = Off`**) de la consulta `ColorFormats`.

#### 📊 Tabla Auxiliar: `ColorFormats` *(Carga deshabilitada)*
| Color | Background Color Format | Font Color Format |
| :--- | :---: | :---: |
| Black | `#000000` | `#FFFFFF` |
| Blue | `#0000FF` | `#FFFFFF` |
| Grey | `#808080` | `#FFFFFF` |
| Multi | `#FFFFFF` | `#000000` |
| NA | `#FFFFFF` | `#000000` |
| Red | `#FF0000` | `#FFFFFF` |
| Silver | `#C0C0C0` | `#000000` |
| Silver/Black | `#808080` | `#FFFFFF` |
| White | `#FFFFFF` | `#000000` |
| Yellow | `#FFFF00` | `#000000` |

#### 📊 Tabla Resultante: `Product` (Muestra de registros combinados)
| ProductKey | Product | Standard Cost | Color | Subcategory | Category | Background Color Format | Font Color Format |
| :---: | :--- | :---: | :---: | :--- | :--- | :---: | :---: |
| 214 | Sport-100 Helmet, Red | $13.09 | Red | Helmets | Accessories | `#FF0000` | `#FFFFFF` |
| 217 | Sport-100 Helmet, Black | $13.09 | Black | Helmets | Accessories | `#000000` | `#FFFFFF` |
| 222 | Sport-100 Helmet, Blue | $13.09 | Blue | Helmets | Accessories | `#0000FF` | `#FFFFFF` |
| 310 | Road-150 Red, 62 | $2,171.29 | Red | Road Bikes | Bikes | `#FF0000` | `#FFFFFF` |
| 320 | Road-650 Red, 58 | $486.71 | Red | Road Bikes | Bikes | `#FF0000` | `#FFFFFF` |
| 344 | Mountain-100 Silver, 38 | $1,912.15 | Silver | Mountain Bikes | Bikes | `#C0C0C0` | `#000000` |
| 352 | Mountain-200 Silver, 38 | $1,117.86 | Silver | Mountain Bikes | Bikes | `#C0C0C0` | `#000000` |
| 463 | Half-Finger Gloves, S | $9.16 | Black | Gloves | Clothing | `#000000` | `#FFFFFF` |
| 477 | Water Bottle - 30 oz. | $1.87 | NA | Bottles and Cages | Accessories | `#FFFFFF` | `#000000` |
| 561 | Touring-1000 Yellow, 46 | $1,481.94 | Yellow | Touring Bikes | Bikes | `#FFFF00` | `#000000` |

---

### Ejercicio 4: Configuración de la consulta `Reseller`
1. Se renombró `DimReseller` a **`Reseller`** y se conservaron `ResellerKey`, `BusinessType`, `ResellerName` y `DimGeography`.
2. Se expandió `DimGeography` extrayendo **`City`**, **`StateProvinceName`** y **`EnglishCountryRegionName`**.
3. Se reemplazó el valor erróneo `"Ware House"` por **`"Warehouse"`** en la columna `BusinessType`.
4. Se renombraron las columnas a **`Business Type`**, **`Reseller`**, **`State-Province`** y **`Country-Region`** (701 filas).

#### 📊 Tabla Resultante: `Reseller` (Muestra de registros limpios)
| ResellerKey | Business Type | Reseller | City | State-Province | Country-Region |
| :---: | :--- | :--- | :--- | :--- | :--- |
| 1 | Value Added Reseller | A Bike Store | Seattle | Washington | United States |
| 2 | Specialty Bike Shop | Progressive Sports | Renton | Washington | United States |
| 3 | Warehouse | Advanced Bike Components | Irving | Texas | United States |
| 4 | Value Added Reseller | Modular Cycle Systems | Austin | Texas | United States |
| 5 | Warehouse | Metropolitan Sports Supply | Fremont | California | United States |
| 6 | Specialty Bike Shop | Aerobic Exercise Company | Toronto | Ontario | Canada |
| 7 | Warehouse | Associated Bikes | Burlingame | California | United States |
| 8 | Value Added Reseller | Exemplary Cycles | London | England | United Kingdom |

---

### Ejercicio 5: Configuración de la consulta `Region`
1. Se renombró `DimSalesTerritory` a **`Region`**.
2. Se filtró `SalesTerritoryAlternateKey <> 0` para eliminar la fila *Corporate HQ*.
3. Se conservaron y renombraron las columnas **`SalesTerritoryKey`**, **`Region`**, **`Country`** y **`Group`** (10 regiones).

#### 📊 Tabla Resultante: `Region` (10 filas completas)
| SalesTerritoryKey | Region | Country | Group |
| :---: | :--- | :--- | :--- |
| 1 | Northwest | United States | North America |
| 2 | Northeast | United States | North America |
| 3 | Central | United States | North America |
| 4 | Southwest | United States | North America |
| 5 | Southeast | United States | North America |
| 6 | Canada | Canada | North America |
| 7 | France | France | Europe |
| 8 | Germany | Germany | Europe |
| 9 | Australia | Australia | Pacific |
| 10 | United Kingdom | United Kingdom | Europe |

---

### Ejercicio 6: Configuración de la consulta `Sales`
1. Se renombró `FactResellerSales` a **`Sales`**.
2. Se conservaron las columnas `SalesOrderNumber`, `OrderDate`, `ProductKey`, `ResellerKey`, `EmployeeKey`, `SalesTerritoryKey`, `OrderQuantity`, `UnitPrice`, `TotalProductCost`, `SalesAmount` y `DimProduct`.
3. Se expandió `DimProduct` para obtener **`StandardCost`**.
4. Se agregó la columna personalizada **`Cost`** para corregir los valores nulos de `TotalProductCost`:
   ```powerquery
   if [TotalProductCost] = null then [OrderQuantity] * [StandardCost] else [TotalProductCost]
