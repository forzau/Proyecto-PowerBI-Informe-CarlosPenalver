# Formula 1 Performance Analytics — Power BI

Dashboard interactivo desarrollado en **Power BI** para analizar el rendimiento de pilotos y constructores de **Fórmula 1 entre 2022 y 2025**.

El proyecto cubre el proceso completo de Business Intelligence: **transformación de datos con Power Query, modelado dimensional, creación de medidas DAX, inteligencia temporal, diseño de visualizaciones e implementación de navegación, marcadores y drillthrough**.

El objetivo es ir más allá de las clasificaciones finales y permitir explorar cómo evolucionó el rendimiento competitivo durante este periodo, relacionando resultados, clasificación, fiabilidad, ritmo de carrera y estrategia de paradas.

---

## Objetivos

El dashboard permite responder preguntas como:

* ¿Cómo evolucionó el rendimiento de pilotos y constructores entre 2022 y 2025?
* ¿Qué pilotos y equipos acumularon más puntos, victorias, podios y poles?
* ¿Cómo se relaciona el rendimiento en clasificación con el resultado de carrera?
* ¿Qué pilotos ganan o pierden más posiciones durante las carreras?
* ¿Qué importancia tiene la fiabilidad en los resultados?
* ¿Cómo evoluciona el ritmo de carrera vuelta a vuelta?
* ¿Qué relación observable existe entre las paradas en boxes y los cambios de ritmo?
* ¿Qué ocurrió en un Gran Premio concreto?

---

## Dataset

Los datos proceden del dataset público **Formula 1 Race Data**, disponible en Kaggle y basado en datos históricos de Ergast/Jolpica.

**Fuente:**
https://www.kaggle.com/datasets/jtrotman/formula-1-race-data

El dataset original contiene **14 archivos CSV** con información histórica de Fórmula 1. Para este proyecto se seleccionaron únicamente las tablas necesarias y se limitó el análisis al periodo **2022–2025**, correspondiente a **92 Grandes Premios**.

Entre los datos utilizados se encuentran:

* carreras;
* pilotos;
* constructores;
* circuitos;
* resultados;
* clasificación;
* carreras Sprint;
* tiempos por vuelta;
* paradas en boxes;
* estados y abandonos.

---

## Tecnologías utilizadas

* **Power BI Desktop**
* **Power Query / M**
* **DAX**
* Modelado dimensional
* Inteligencia temporal
* Diseño de dashboards
* Data storytelling

---

## Preparación de datos con Power Query

Los archivos originales contienen información de toda la historia de la Fórmula 1, por lo que la primera transformación consistió en limitar los datos al periodo **2022–2025**.

Entre las principales operaciones realizadas se encuentran:

* filtrado de temporadas;
* corrección de tipos de datos;
* eliminación de columnas no relevantes;
* normalización de valores nulos representados como `\N`;
* combinación de consultas mediante joins;
* filtrado de dimensiones y tablas de hechos únicamente a los registros utilizados;
* creación de nombres completos de pilotos;
* conversión de tiempos almacenados como texto o milisegundos a segundos;
* incorporación de `constructorId` en las tablas de vueltas y pit stops;
* creación de columnas auxiliares necesarias para el análisis.

Algunos campos derivados relevantes son:

* `driverName`
* `fastestLapSeconds`
* `lapTimeSeconds`
* `pitStopSeconds`
* tiempos de clasificación Q1, Q2 y Q3 expresados en segundos.

---

## Modelo de datos

El modelo sigue una arquitectura dimensional con separación entre **dimensiones y tablas de hechos**.

### Dimensiones

* `DIM_Calendar`
* `DIM_Races`
* `DIM_Drivers`
* `DIM_Constructors`
* `DIM_Circuits`
* `DIM_Status`

### Tablas de hechos

* `FACT_Results`
* `FACT_Qualifying`
* `FACT_SprintResults`
* `FACT_PitStops`
* `FACT_LapTimes`

Además, se utiliza una tabla específica:

* `MEDIDAS`

para centralizar las medidas DAX.

`DIM_Calendar` se relaciona con `DIM_Races` mediante la fecha, mientras que `DIM_Races` actúa como una de las principales dimensiones desde las que se propaga el contexto de carrera hacia las diferentes tablas de hechos.

Esta arquitectura permite combinar diferentes granularidades —resultados, clasificación, vueltas y pit stops— manteniendo un contexto analítico coherente.

---

## DAX

El informe contiene aproximadamente **40 medidas DAX**, organizadas por bloques funcionales.

### Resultados

* Puntos totales
* Victorias
* Podios
* Poles
* Carreras disputadas
* Participaciones

### Rendimiento y fiabilidad

* Abandonos
* Tasa de abandono
* Cambio medio de posición
* Cambio medio entre clasificación y carrera
* Tasa de acceso a Q3

### Inteligencia temporal

* Puntos año anterior
* Variación interanual de puntos
* Puntos acumulados de temporada

### Pit stops

* Total de pit stops
* Duración media
* Duración mediana
* Pit stop más rápido
* Pit stops por participación

### Ritmo de carrera

* Mediana de tiempo por vuelta
* Mejor mediana de carrera
* Gap respecto al mejor ritmo
* Ritmo relativo
* Tiempo medio por vuelta
* Desviación del tiempo por vuelta
* Mejor vuelta
* Vueltas registradas

También se desarrollaron medidas auxiliares para resolver contextos entre tablas de hechos de distinta granularidad, utilizando técnicas como `TREATAS`.

---

## Informe

El dashboard está compuesto por **cinco páginas**, cada una diseñada para responder una pregunta analítica diferente.

### 1. Resumen Ejecutivo

Proporciona una visión rápida del campeonato mediante los principales indicadores.

Incluye:

* puntos;
* victorias;
* podios;
* poles;
* tasa de abandono;
* Top 5 pilotos;
* Top 5 constructores;
* evolución acumulada del campeonato.

Dispone de filtros por temporada, piloto y constructor.

---

### 2. Evolución

Analiza cómo cambia el rendimiento competitivo durante el periodo 2022–2025.

La página permite alternar mediante **marcadores** entre dos perspectivas:

* Pilotos
* Constructores

Incluye:

* evolución de puntos por temporada;
* variación interanual;
* ranking competitivo mediante Ribbon Chart;
* victorias y podios por temporada.

Esta página concentra gran parte de la **inteligencia temporal** del modelo.

---

### 3. Rendimiento

Estudia cómo se traduce el rendimiento en clasificación al resultado final de carrera.

Incluye indicadores como:

* Tasa Q3
* Cambio medio clasificación-carrera
* Cambio medio de posición
* Tasa de abandono

Los principales análisis son:

* relación entre clasificación y rendimiento en carrera;
* ganancia o pérdida de posiciones por Gran Premio;
* motivos de abandono.

Esto permite diferenciar rendimiento puro, capacidad de progresión durante la carrera y fiabilidad.

---

### 4. Ritmo y Estrategia

Profundiza en el desarrollo interno de una carrera.

Incluye:

* mediana del tiempo por vuelta;
* gap respecto al mejor ritmo;
* ritmo relativo;
* total de pit stops.

El análisis principal utiliza la **mediana del tiempo de vuelta** para representar el ritmo sostenido y reducir la influencia de valores extremos.

La página combina:

* evolución del ritmo vuelta a vuelta;
* distribución temporal de las paradas en boxes;
* duración mediana de los pit stops por piloto.

Para relacionar las tablas de vueltas y paradas se utilizaron medidas DAX específicas con `TREATAS`.

El objetivo es identificar coincidencias temporales entre cambios de ritmo y estrategia sin interpretar automáticamente estas relaciones como causalidad.

---

### 5. Detalle GP

Página de **drillthrough** destinada al análisis de un Gran Premio concreto.

Permite pasar desde visualizaciones de las páginas anteriores a un nivel de mayor granularidad para contextualizar eventos o resultados especialmente interesantes.

El análisis integra información relacionada con:

* resultado de carrera;
* posición de salida y llegada;
* puntos;
* clasificación;
* cambios de posición;
* estado final y abandonos;
* ritmo;
* pit stops.

La página permanece fuera de la navegación principal y se utiliza como destino de exploración mediante drillthrough.

---

## Interactividad

El dashboard incorpora diferentes mecanismos de interacción de Power BI:

### Filtros

Segmentadores de:

* temporada;
* Gran Premio;
* piloto;
* constructor.

Su comportamiento se adapta a las necesidades analíticas de cada página.

### Navegación

Un navegador horizontal permite desplazarse entre las páginas principales manteniendo una estructura consistente.

### Marcadores

La página **Evolución** utiliza marcadores para alternar entre el análisis de:

* pilotos;
* constructores.

### Drillthrough

Desde determinados análisis se puede acceder a **Detalle GP**, manteniendo el contexto del Gran Premio seleccionado.

Esto permite pasar de una anomalía o resultado detectado en una vista agregada a su análisis detallado.

---

## Diseño

Se creó un tema personalizado inspirado en la identidad visual del motorsport.

Paleta principal:

| Elemento              | Color     |
| --------------------- | --------- |
| Fondo principal       | `#0F1115` |
| Superficies           | `#1A1D23` |
| Superficie secundaria | `#242831` |
| Texto principal       | `#F5F5F5` |
| Texto secundario      | `#A7ABB4` |
| Acento                | `#E10600` |
| Separadores           | `#343A46` |

La interfaz utiliza una estética oscura y técnica, reservando el rojo como color de acento para evitar saturar las visualizaciones.

---

## Decisiones analíticas destacadas

### Uso de la mediana para estudiar ritmo

Los tiempos de vuelta pueden verse afectados por pit stops, banderas, Safety Car, tráfico o incidentes.

Por ello, para representar el ritmo sostenido se prioriza la **mediana del tiempo por vuelta** frente a la media.

### Comparación relativa del ritmo

Se desarrollaron medidas para comparar el ritmo de cada piloto con la mejor mediana registrada dentro del mismo Gran Premio:

```DAX
Gap Ritmo vs Mejor =
[Mediana Tiempo Vuelta] - [Mejor Mediana Carrera]
```

y su versión relativa:

```DAX
Ritmo Relativo =
DIVIDE(
    [Mediana Tiempo Vuelta] - [Mejor Mediana Carrera],
    [Mejor Mediana Carrera]
)
```

### Contexto entre tablas de hechos

`FACT_LapTimes` y `FACT_PitStops` tienen granularidades diferentes.

Para determinados visuales fue necesario trasladar explícitamente el contexto de vuelta utilizando DAX y `TREATAS`, evitando crear relaciones incorrectas únicamente para resolver una visualización.

### Correlación no implica causalidad

La coincidencia entre una parada en boxes y una modificación posterior del ritmo permite identificar patrones interesantes, pero los datos disponibles no permiten afirmar que una parada sea necesariamente la causa del cambio observado.

---

## Cómo ejecutar el proyecto

### Requisitos

* Power BI Desktop

### Uso

1. Clonar o descargar este repositorio.
2. Abrir el archivo `.pbix` con Power BI Desktop.
3. Utilizar los filtros y navegación disponibles para explorar el informe.

En caso de querer actualizar los datos desde los CSV originales, será necesario descargar el dataset de Kaggle y actualizar las rutas de origen correspondientes en Power Query.

---

## Limitaciones

El análisis está limitado a las temporadas **2022–2025**.

Además, el dataset utilizado contiene información de resultados, clasificación, vueltas y pit stops, pero no incluye toda la telemetría disponible internamente para los equipos de Fórmula 1.

Por ello, el dashboard está orientado al **análisis descriptivo y comparativo del rendimiento** y no pretende explicar causalmente todos los factores que determinan el resultado de una carrera.

---

## Competencias aplicadas

Este proyecto demuestra conocimientos prácticos en:

* limpieza y transformación de datos;
* Power Query;
* modelado dimensional;
* diseño de esquemas analíticos;
* DAX;
* inteligencia temporal;
* gestión del contexto de filtro;
* análisis de distintas granularidades;
* visualización de datos;
* diseño de dashboards;
* interacción mediante filtros, marcadores y drillthrough;
* storytelling con datos.

---

## Contexto

Proyecto desarrollado como **Trabajo Final del módulo de Power BI** dentro de un Máster en Ciencia de Datos e Inteligencia Artificial.

El proyecto fue elegido alrededor de Fórmula 1 con el objetivo de aplicar técnicas de Business Intelligence sobre un dominio deportivo con datos suficientemente ricos para analizar rendimiento, evolución, fiabilidad, ritmo y estrategia.
