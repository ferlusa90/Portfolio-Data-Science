
Desarrollado por Fernando Emmanuel Sanchez Luna Científico de Datos | Analista de Datos

LinkedIn: linkedin.com/in/fernando-sanchez-759a93168

Correo Electrónico: fer.luna90@gmail.com
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Monitoreo y Análisis de Emisiones de CO2 en Vehículos (2015 - 2026)
> **Proyecto de Portafolio - Ciencia de Datos / Análisis de Datos**

Este repositorio contiene un tablero interactivo desarrollado en **Power BI** enfocado en el análisis de las emisiones de dióxido de carbono ($CO_2$) de diferentes tipos de vehículos (Combustión Interna, Eléctricos, Híbridos e Hidrógeno) en el periodo comprendido entre 2015 y 2026.

El objetivo principal es identificar patrones de contaminación por marcas, tipos de combustible y evolución temporal para evaluar la transición hacia una movilidad más sustentable.

---

## 📊 Vista General del Dashboard

El reporte consta de tres secciones principales (pestañas):

1. **KPI CO2:** Resumen ejecutivo con métricas clave dinámicas (CO2 Máximo detectado, Promedio de CO2 por milla) y segmentadores interactivos por tipo de combustible.
2. **Emisión Total:** Análisis comparativo de emisiones acumuladas por marca automotriz y distribución del promedio por tipo de combustible utilizando mapas de árbol (*Treemap*).
3. **Emisión por Vehículo:** Vista granular y temporal que analiza la evolución del promedio de $CO_2$ año a año, permitiendo filtrar específicamente por marca y modelo (ej. *Audi - A4 quattro*).

---

## 🛠️ Tecnologías y Herramientas Utilizadas
* **Power BI Desktop:** Diseño de la interfaz de usuario con maquetación en fondo oscuro (*Dark Mode*) para mejorar el contraste y la legibilidad.
* **Power Query:** Extracción, transformación y carga (ETL) de los datos, incluyendo limpieza de estructuras, tipado y filtrado de registros.
* **DAX (Data Analysis Expressions):** Modelado analítico avanzado mediante el uso de variables, iteradores y funciones de manipulación de contexto de filtrado.

---

## 💾 Origen y Estructura de los Datos

Los datos analizados fueron obtenidos desde **Kaggle**. El conjunto de datos cuenta con una estructura robusta ideal para procesos de analítica predictiva y descriptiva:
* **Volumen:** +15,000 filas (registros de vehículos).
* **Dimensiones:** 14 columnas que incluyen variables técnicas como Marca (`Make`), Modelo (`Model`), Tipo de Combustible (`Fuel_Type`), Tipo de Transmisión (`Transmission`) y Año de fabricación (`Year`).

---

## 🧼 Proceso de Preparación de Datos (ETL)

La calidad del set de datos original `EV_vs_ICE_Vehicle_Specs_2015_2026` fue garantizada en el editor de **Power Query** mediante el siguiente flujo de trabajo:
1. **Origen:** Conexión y lectura del archivo fuente.
2. **Encabezados promovidos:** Conversión de la primera fila en los metadatos de las columnas.
3. **Tipo cambiado:** Asignación estricta de tipos de datos (Numéricos para variables de emisión/años, texto para categorías).
4. **Filas filtradas:** Limpieza profunda de registros duplicados, nulos o con inconsistencias operativas.

---

## 🧠 Modelado Analítico con DAX

Para lograr un comportamiento dinámico y responder de forma eficiente a los filtros cruzados del dashboard, se desarrollaron medidas explícitas avanzadas utilizando buenas prácticas de optimización:

### 1. Promedio de CO2 por Marca (Iteradora con contexto dinámico)
Calcula el promedio respetando los filtros activos sobre la dimensión de marcas:
```dax
Promedio CO2 por Marca = 
AVERAGEX(
    VALUES( EV_vs_ICE_Vehicle_Specs_2015_2026[Make] ),
    CALCULATE( AVERAGE( EV_vs_ICE_Vehicle_Specs_2015_2026[CO2_Emissions_g_per_mile] ) )
)

###2. Máxima Emisión de CO2 Registrada
Aísla el valor más alto de emisiones considerando la segmentación del usuario en la vista actual

CO Mayor = 
VAR TopVehiculo =
    TOPN(
        1,
        ALLSELECTED(EV_vs_ICE_Vehicle_Specs_2015_2026),
        EV_vs_ICE_Vehicle_Specs_2015_2026[CO2_Emissions_g_per_mile],
        DESC
    )
RETURN
    MAXX(
        TopVehiculo,
        EV_vs_ICE_Vehicle_Specs_2015_2026[CO2_Emissions_g_per_mile]
    )

###3. Identificación del Vehículo con Mayor Impacto Contaminante
Devuelve dinámicamente el nombre de la Marca y el Modelo del vehículo que registra el pico más alto de emisiones según los filtros aplicados:
Vehiculo Mayor CO2 = 
VAR TopVehiculo =
    TOPN(
        1,
        ALLSELECTED(EV_vs_ICE_Vehicle_Specs_2015_2026),
        EV_vs_ICE_Vehicle_Specs_2015_2026[CO2_Emissions_g_per_mile], DESC,
        EV_vs_ICE_Vehicle_Specs_2015_2026[Make], ASC,
        EV_vs_ICE_Vehicle_Specs_2015_2026[Model], ASC,
        EV_vs_ICE_Vehicle_Specs_2015_2026[Transmission], ASC
    )
RETURN
MAXX(
    TopVehiculo,
    EV_vs_ICE_Vehicle_Specs_2015_2026[Make]
        & " - "
        & EV_vs_ICE_Vehicle_Specs_2015_2026[Model]
)

📉 Insights Clave Extraídos del Negocio
Tendencia de Descarbonización: Se identifica visualmente un punto de inflexión a partir de 2020 con una caída pronunciada en el promedio de emisiones globales hacia 2025. Esto valida estadísticamente el impacto de la introducción de flotas eléctricas e híbridas en el mercado automotriz.

Concentración por Fabricante: Marcas con líneas de producción enfocadas en vehículos de alta potencia o carga (como Jeep, Ford y Chevrolet) concentran el mayor volumen acumulado de emisiones en el periodo analizado.

Matriz de Combustible: Aunque las tecnologías alternativas ganan terreno, el Diesel y la Gasolina de grado medio (Midgrade) retienen los promedios de emisiones por milla más severos, destacando la necesidad de regulaciones sobre estos segmentos.


