# Análisis de Churn de Clientes - Telecom X

Este proyecto realiza un proceso completo de ETL y Análisis Exploratorio de Datos (EDA) para entender los factores que influyen en la pérdida de clientes (churn) en Telecom X.

## 📌 Extracción

1. **Carga de datos**: Se obtiene el archivo JSON crudo desde GitHub:

   ```python
   url = 'https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/main/TelecomX_Data.json'
   df = pd.read_json(url)
   ```
2. **Inspección inicial**:

   ```python
   df.head()       # Primeras 5 filas
   df.info()       # Tipos y nulos
   df.shape        # Dimensiones
   ```

## 🔧 Transformación

3. **Desanidar** columnas con diccionarios (`customer`, `phone`, `internet`, `account`):

   ```python
   from pandas import json_normalize
   dict_cols = [c for c in df.columns if df[c].apply(lambda x: isinstance(x, dict)).any()]
   for col in dict_cols:
       flat = json_normalize(df[col])
       # Renombrar y unir
       ...
   ```
4. **Estandarizar nombres** de columnas: se reemplazan espacios, mayúsculas y caracteres especiales por snake\_case.
5. **Conversión de tipos**:

   * `churn` a binario (0/1)
   * `cargo_mensual` y `cargos_totales` a numérico
   * Eliminación de duplicados y filas con valores críticos faltantes.
6. **Feature engineering**: creación de `cargo_diario` dividiendo el cargo mensual por 30.

## 📊 Carga y Análisis

7. **Estadísticas descriptivas**:

   ```python
   df[['cargo_mensual','cargos_totales','cargo_diario']].describe().T
   df[['cargo_mensual','cargos_totales','cargo_diario']].median()
   ```
8. **Distribución de churn**: gráfico de barras con proporción de clientes que abandonan vs. permanecen.
9. **Churn por categorías** (`Género`, `Contrato`, `Internet`):

   ```python
   df.groupby('gender')['churn'].mean() * 100
   ```
10. **Análisis de segmentos y correlación**:

    * Promedio de cargo mensual por tipo de contrato.
    * Tabla de contingencia de churn por tipo de Internet.
    * Matriz de correlación entre variables clave.

## 📄 Informe Final

* **Tasa global de churn**: XX.XX%
* **Contratos mensuales**: XX.XX% de churn.
* **Clientes con fibra óptica**: XX.XX% de churn.
* **Correlación** entre churn y cargo mensual: X.XX.
* **Insight clave**: clientes nuevos (<12 meses) tienen mayor probabilidad de abandono.

---

**Requisitos**:

* Python 3.7+
* pandas
* matplotlib
* seaborn

