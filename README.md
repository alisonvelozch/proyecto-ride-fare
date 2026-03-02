## 📌 Descripción

Estudio de precios y demanda de viajes en plataformas digitales como **Uber y Lyft**, considerando la influencia del **clima** y la **ubicación geográfica** en el comportamiento de los usuarios en la ciudad de Boston.

---

## 🎯 Objetivo

Comprender la estructura, calidad y contenido de los datasets para identificar problemas de calidad de datos (nulos, duplicados, outliers) y visualizar las principales características de las variables, con miras a análisis y modelados posteriores.

---

## 📁 Estructura del Proyecto

```
proyecto-ride-fare/
│
├── PROYECTO_RIDE_FARE_.ipynb    ← Notebook principal con todo el análisis
├── PFDA_rides.csv               ← Dataset de viajes (693,071 registros)
├── PFDA_weather.csv             ← Dataset de condiciones climáticas (6,276 registros)
└── README.md
```

> ⚠️ Los archivos CSV pesan más de 25MB, descárgalos desde:  
> [📂 Google Drive - Datasets](https://drive.google.com/drive/folders/1ugigVlvpzbfM354iBIh-QWe7lJyLdDAk)

---

## 📊 Contenido del Análisis

### 1. Dataset Rides — 693,071 registros | 10 columnas
| Variable | Descripción |
|----------|-------------|
| `distance` | Distancia del viaje (millas) |
| `cab_type` | Tipo de servicio (Uber / Lyft) |
| `time_stamp` | Momento del viaje |
| `source` / `destination` | Zona de origen y destino |
| `price` | Precio del viaje |
| `surge_multiplier` | Factor de tarifa dinámica |
| `id` / `product_id` | Identificadores del viaje |
| `name` | Modalidad del servicio |

### 2. Dataset Weather — 6,276 registros | 8 columnas
| Variable | Descripción |
|----------|-------------|
| `temp` | Temperatura (°F) |
| `location` | Lugar de medición |
| `clouds` | Nubosidad |
| `pressure` | Presión atmosférica (hPa) |
| `rain` | Precipitación (mm) |
| `humidity` | Humedad relativa |
| `wind` | Velocidad del viento |
| `time_stamp` | Fecha y hora del registro |

---

## 🔍 Hallazgos Principales

### Calidad de Datos
- **Rides:** Sin duplicados. Valores nulos en `price` pertenecen exclusivamente al servicio Taxi (7.95%). Se creó la columna `price_available`.
- **Weather:** Sin duplicados. La columna `rain` presenta 85.76% de nulos — imputados con `0.0 mm` ya que representan ausencia de lluvia, no errores de sensor.
- **Outliers mantenidos** en ambos datasets al confirmarse que representan comportamientos reales (viajes largos, tarifas dinámicas, lluvias leves).

### Distribución de Variables
- Los precios se concentran entre **$2.50 y $97.50**, con mediana de **$13.50**.
- La mayoría de viajes cubre distancias de **1 a 3 millas**.
- Uber registra más viajes en total (385,663 vs 307,408), aunque Lyft domina en algunos rangos de precio.
- Las temperaturas se concentran entre **34°F y 44°F**.

---

## 📍 Propuesta: Segmentación Geográfica por Condiciones Ambientales

### Integración de Datasets
Se vincularon ambos datasets por **ubicación geográfica** (`source`/`destination` ↔ `location`) con un margen de tolerancia de **2 horas** para asignar el clima más cercano a cada viaje.

### Categorización de Zonas
| Zona | Barrios incluidos |
|------|------------------|
| 🏢 Comercial | Back Bay, Financial District, Theatre District |
| 🎓 Universitaria | Boston University, Northeastern University, Fenway |
| 🏘️ Residencial–Central | Beacon Hill, North End, Haymarket Square, West End, North Station, South Station |

### Preguntas de Investigación y Resultados

**1. ¿El clima influye en la elección de vehículo?**  
No significativamente. Los servicios económicos dominan con ~50% en todas las condiciones climáticas. Solo en clima nublado se observa un pequeño aumento (+1%) en vehículos accesibles.

**2. ¿Cómo varía el precio y distancia según el clima por zona?**  
- La **lluvia** reduce la distancia recorrida en casi todas las zonas.
- Los cambios de precio se mantienen dentro de **±$1**, sin variaciones abruptas.
- La zona universitaria es la más costosa; la residencial–central la más económica y estable.

**3. ¿Existen "corredores climáticos"?**  
Sí. Se identificaron **10 rutas** con alta sensibilidad al clima:

| Ruta | Variación de precio |
|------|-------------------|
| Theatre District → Boston University | **+$5.77** ⚠️ |
| South Station → Back Bay | +$3.43 |
| Back Bay → South Station | +$2.56 |
| Financial District → Fenway | +$2.43 |
| Back Bay → Northeastern University | +$2.12 |

**4. ¿La disponibilidad de Uber vs Lyft varía por zona o clima?**  
No de forma significativa. Uber mantiene una ligera ventaja constante (~52% vs ~48%). La única excepción es el **clima nublado**, donde Uber alcanza ~53.56% en todas las zonas.

---

## 💡 Conclusiones y Recomendaciones

- Mantener y reforzar la flota de **vehículos económicos** como categoría dominante.
- Diseñar **estrategias zonales para lluvia**: más oferta en zonas comerciales, estándar en universitarias, reducida en residenciales.
- **Priorizar gestión operativa en corredores climáticos** sensibles (Theatre District–Boston University, Back Bay–South Station).
- Implementar **redistribución dinámica de conductores** ante pronósticos de clima adverso, especialmente en días nublados donde Uber amplía su ventaja.

---

## ▶️ Cómo Ejecutar

1. Abre el notebook en [Google Colab](https://colab.research.google.com/)
2. Sube los archivos `PFDA_rides.csv` y `PFDA_weather.csv`
3. Ejecuta las celdas en orden

---

## 🛠️ Librerías Utilizadas

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```
