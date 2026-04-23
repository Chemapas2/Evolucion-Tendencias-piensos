# Comparador de tendencias de formulación de piensos

Aplicación en **Streamlit** para comparar tendencias mensuales a partir de archivos Excel con el mismo formato que el adjunto de ejemplo.

## Qué hace

La app permite:

- Cargar entre **2 y 12 archivos `.xlsx`**.
- Detectar automáticamente los bloques de formulación contenidos en cada archivo.
- Seleccionar **producto**, **nutrientes** e **ingredientes**.
- Mostrar, cuando se selecciona un producto:
  - la evolución temporal del **precio del producto**,
  - la evolución temporal de los **nutrientes seleccionados**,
  - la evolución temporal de los **ingredientes seleccionados** en ese producto.
- Mostrar, cuando se deja vacío el producto y se selecciona **un único ingrediente**:
  - la evolución temporal de su **precio estimado**,
  - una **estimación relativa de consumo** basada en las toneladas presentes en las formulaciones cargadas.
- Usar **líneas** cuando se comparan varias variables y **barras** cuando solo se representa una variable.
- Descargar el análisis actual en **Excel** y el resumen en **texto plano**.
- Mostrar este **README dentro de la propia app**.

## Supuestos del parser

La app está diseñada para libros Excel con estas características:

- una hoja principal,
- contenido dispuesto en una sola columna como texto formateado,
- bloques repetidos que incluyen al menos:
  - `Specification:`
  - `INCLUDED RAW MATERIALS`
  - `ANALYSIS`
  - opcionalmente `RAW MATERIAL SENSITIVITY`
- el periodo del archivo se infiere a partir de cadenas como `Marzo 26`, `Abril 26`, etc.

## Limitación importante sobre el consumo de ingredientes

La app **no calcula consumo real de fábrica o compras**, porque ese dato no existe en los archivos de formulación por sí solos.

Lo que sí calcula es un **consumo relativo estimado**, definido como la suma de las toneladas del ingrediente presentes en las formulaciones cargadas para cada periodo. Ese indicador sirve para comparar tendencia interna entre archivos, pero no debe interpretarse como consumo industrial real sin datos adicionales de producción.

## Estructura del proyecto

```text
main.py
requirements.txt
README.md
```

## Instalación

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

En Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Ejecución

```bash
streamlit run main.py
```

## Flujo de uso

1. Carga entre 2 y 12 archivos del mismo formato.
2. Selecciona un **producto** para ver su evolución temporal.
3. Añade los **nutrientes** y **ingredientes** que quieras comparar.
4. Para la vista centrada en ingrediente:
   - deja el producto vacío,
   - selecciona un único ingrediente.
5. Descarga el análisis en Excel o el informe de texto.

## Métricas extraídas

### Producto

- `cost_per_tonne`
- `total_tonnes`
- código y nombre de producto
- código de especificación

### Ingredientes

- coste medio del ingrediente
- porcentaje de inclusión
- kilos por lote
- toneladas por lote
- límites mínimo y máximo cuando existen

### Nutrientes

- nombre del nutriente
- valor de `level`

## Notas técnicas

- La app usa `openpyxl` para leer los libros Excel.
- Los gráficos se generan con `plotly`.
- La exportación a Excel usa `pandas` + `XlsxWriter`.
- Si se cargan dos archivos con la misma etiqueta de periodo, la app añade el nombre del fichero para distinguirlos en el eje temporal.

## Posibles ampliaciones

- normalización adicional de nombres de ingredientes entre plantas o códigos alternativos,
- incorporación de volúmenes reales de fabricación para transformar el consumo relativo en consumo real,
- filtros por familias de productos,
- comparación simultánea de varios productos.
