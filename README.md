# Modelado vehículos eléctricos
# Caso de Negocio
## Descripción del problema
El crecimiento acelerado del parque automotor en Colombia genera impactos significativos en movilidad, contaminación, infraestructura y planificación urbana. Sin embargo, el país también está experimentando un aumento en la adopción de vehículos eléctricos e híbridos, lo cual abre oportunidades para políticas de electromovilidad y reducción de emisiones.

El problema central es entender cómo crece el parque automotor total y qué proporción corresponde a vehículos eléctricos e híbridos, con el fin de predecir tendencias futuras y evaluar el impacto potencial de la transición energética.
## Objetivo del proyecto 

* Analizar el crecimiento histórico del parque automotor colombiano.

* Comparar la evolución de los vehículos eléctricos e híbridos frente al total.

* Construir un modelo predictivo del crecimiento futuro.

* Exponer los resultados mediante un endpoint y una app de visualización.

* Generar insumos para decisiones de política pública, movilidad sostenible e infraestructura de carga.

# Relación Beneficio/Coste - Análisis económico
## Justificación mediante ahorros
La adopción de vehículos eléctricos reduce costos operativos por mantenimiento y combustible.
El análisis permite estimar:

* Ahorros por transición a EV/Híbridos.

* Reducción de emisiones y costos asociados a salud pública.

* Optimización de infraestructura de carga.

## Retorno de Inversión (ROI)
Se compara:

* Costo de implementación del proyecto (procesamiento Big Data, modelado, visualización).

* Beneficios proyectados por:

* Ahorros operativos.

* Reducción de emisiones.

* Incremento en la adopción de tecnologías limpias.

## Mayores ingresos
El análisis permite identificar:

* Ciudades con mayor potencial de adopción EV.

* Oportunidades para empresas de energía, concesionarios y fabricantes.

* Proyecciones de crecimiento del mercado automotor.

# Arquitectura

                ┌──────────────────────────┐
                │      RUNT 2.0 Datos      │
                │  - Parque automotor      │
                │  - EV/Híbridos           │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │         BRONZE           │
                │  Datos crudos (CSV/XLSX) │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │         SILVER           │
                │ Limpieza y transformación│
                │  - Homologación ciudades │
                │  - Cálculo proporciones  │
                │  - Crecimiento anual     │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │          GOLD            │
                │ Dataset final ciudad-año │
                │  - Total parque          │
                │  - EV/Híbridos           │
                │  - % EV/Híbridos         │
                │  - Crecimiento anual     │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │     Serving Endpoint     │
                │  Exposición del dataset  │
                │  vía API REST            │
                └─────────────┬────────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │         Power BI         │
                │  Dashboard conectado al  │
                │  endpoint (POST + token) │
                └──────────────────────────┘
# PIPELINE Ingesta de datos

## Bronze: 
La información se obtiene de 2 tablas oficiales de Runt, una de vehículos eléctricos en Colombia y otra de Crecimiento del parque automotor. 
<img width="1063" height="535" alt="image" src="https://github.com/user-attachments/assets/ff7e513c-b5e2-426f-aefd-162ddfa4d8ea" />
<img width="1061" height="580" alt="image" src="https://github.com/user-attachments/assets/a6227790-498e-479a-862a-e78b9a15f570" />

## Silver:
Se realiza la limpieza de la información 
<img width="1061" height="581" alt="image" src="https://github.com/user-attachments/assets/c0ac3d11-b578-4986-9d2f-8c318c6fbd96" />
<img width="1063" height="595" alt="image" src="https://github.com/user-attachments/assets/0b52888e-8885-4c3c-90b4-f444f97e7f05" />
## Gold: 
dataset final para modelos y visualización.
<img width="1341" height="589" alt="image" src="https://github.com/user-attachments/assets/62a48a1f-519f-417e-81d6-4ad6bb893206" />

# Modelos
Modelos Implementados
1. RandomForest — Predicción de Conteo por Departamento
Predice el número de vehículos eléctricos/híbridos por departamento y año.
R² test = 0.41 | Features: año + DEPARTAMENTO (one-hot)
Entrenado con datos 2016–2026, validado con split 80/20
Predicciones disponibles para 2027–2029
2. Crecimiento Logístico — Techo de Adopción Nacional
Modela la curva de adopción como proporción del parque total (curva S).
Techo de adopción: 9.51% | Punto de inflexión: año 2024
R² = 0.998 — excelente ajuste a datos históricos
Proyección 2030: ~9.3% del parque automotor nacional
3. Modelo Registrado en Unity Catalog
Modelo: workspace.default.modelo_vehiculos_electricos
Versión: 1 | Alias: Champion
Combina ambos modelos en un PythonModel personalizado
Instrucciones para serving endpoint en el notebook de ML

<img width="1050" height="170" alt="image" src="https://github.com/user-attachments/assets/07f5dd69-722a-4d6d-b133-23572f3a0260" />

## Evolución y proyección 
<img width="552" height="412" alt="Vehículos Eléctricos_Híbridos por Año" src="https://github.com/user-attachments/assets/20f208b9-89f7-4dfe-b0b9-54e2d01644fb" />
<img width="552" height="412" alt="Proporción de Eléctricos en el Parque Total" src="https://github.com/user-attachments/assets/a5726979-683c-4285-a449-7da1356c6125" />
<img width="1112" height="412" alt="Parque Total vs Eléctricos_Híbridos por Año" src="https://github.com/user-attachments/assets/334d4136-c329-42dd-bdea-0e321b49f22c" />

## Análisis geográfico
<img width="552" height="412" alt="Departamentos por Total Eléctricos_Híbridos" src="https://github.com/user-attachments/assets/4d837c7e-8c22-47b4-9b81-bba7b7c3ae3a" />
<img width="552" height="412" alt="Departamentos por Proporción de Adopción" src="https://github.com/user-attachments/assets/b09f1a36-bd4c-40e1-838d-aaf27910810a" />

## Clasificación de municipios
<img width="552" height="412" alt="Departamentos por Proporción de Adopción" src="https://github.com/user-attachments/assets/daff66c4-8f64-48b4-9abc-bfccb4d5d0db" />
<img width="552" height="412" alt="Eléctricos_Híbridos por Nivel de Adopción" src="https://github.com/user-attachments/assets/8673b982-b175-4390-afdf-1e222d264010" />

## Predicciones ML
Esta página muestra las proyecciones del modelo de crecimiento logístico registrado en Unity Catalog.

Parámetros del modelo logístico:
Techo de adopción (L): 9.51%
Punto de inflexión (t₀): 2024
Tasa de crecimiento (k): 0.665
R² del ajuste: 0.998

Modelo RandomForest (conteo por departamento):
R² test: 0.41
Features: año + DEPARTAMENTO (one-hot encoded)
Los datos históricos (2016–2026) provienen de las tablas Gold. Las proyecciones (2027–2030) utilizan el modelo logístico para la proporción nacional y una tasa de crecimiento del 4% anual para el parque automotor total.

<img width="552" height="412" alt="Total Eléctricos_ Histórico vs Proyección" src="https://github.com/user-attachments/assets/6e2d7062-5865-40bb-81fd-d3724a988698" />
<img width="552" height="412" alt="Proporción de Adopción_ Histórico vs Proyección" src="https://github.com/user-attachments/assets/9782e7f5-8c23-43c6-9368-b01afbd009be" />
<img width="552" height="412" alt="Parque Total vs Eléctricos Proyectado" src="https://github.com/user-attachments/assets/7206c1c8-b720-44b6-a7cc-5459e8afa3c1" />

# APP o visualización

## Serving endpoint:
<img width="1342" height="568" alt="image" src="https://github.com/user-attachments/assets/d7598c05-1ddc-4d46-b505-de5e0b268a11" />

<img width="1359" height="668" alt="image" src="https://github.com/user-attachments/assets/d0ba1909-746b-4734-8c75-e97c6e65a09c" />

Este trabajo está presentado por: 
Lucela Montoya Quintero
Lizeth Catalina Pineda Arteaga

