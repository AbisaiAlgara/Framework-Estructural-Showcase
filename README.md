# Framework Estructural Showcase

Repositorio documental de un framework propio para análisis, validación normativa y optimización estructural de edificios de acero.

Este proyecto integra **Python**, **OpenSeesPy**, **DEAP/NSGA-II** y generación automática de reportes técnicos para modelar, analizar y optimizar estructuras metálicas mediante un flujo reproducible y trazable.

> Nota: este repositorio funciona como versión showcase/documental. El código fuente completo no se publica en su totalidad por tratarse de un proyecto académico/profesional en desarrollo.

## Enfoque técnico

* Modelado estructural 3D con OpenSeesPy.
* Análisis modal y respuesta espectral CQC.
* Revisión de derivas, desplazamientos, demanda/capacidad y esbeltez.
* Diseño LRFD de elementos de acero.
* Optimización multiobjetivo mediante NSGA-II.
* Generación automática de reportes en Markdown, CSV y JSON.
* Validación contra software comercial y ejercicios de referencia.

## Caso aplicado

El framework fue aplicado a un edificio metálico de departamentos estudiantiles de cuatro niveles, evaluando alternativas estructurales y seleccionando un sistema con marcos a momento y contravientos Chevron en X.

Resultados principales:

* Peso de acero aproximado: **44.84 t**
* D/C máxima global: **0.793**
* Deriva máxima de servicio: **0.000816**
* Desplazamiento de azotea: **8.35 mm**
* Candidato adoptado: **Rank 55**

Este proyecto integra **Python**, **OpenSeesPy**, **DEAP/NSGA-II** y generación automática de reportes técnicos para modelar, analizar y optimizar estructuras metálicas mediante un flujo reproducible y trazable.

> Nota: este repositorio funciona como versión showcase/documental. El código fuente completo no se publica en su totalidad por tratarse de un proyecto académico/profesional en desarrollo.

## Cómo leer este repositorio

Este repositorio está organizado como una vitrina técnica del framework. No busca publicar todo el código fuente, sino mostrar la arquitectura, documentación, reportes generados y evidencia del flujo computacional.

### Carpetas principales

* `docs/`
  Documentación técnica del framework: arquitectura, UML, convenciones de modelado y criterios de orientación de perfiles.

* `reportes_generados/`
  Ejemplos de salidas automáticas producidas por el framework, incluyendo memorias, CSV, JSON, revisiones LRFD, deflexiones, fuerzas internas y trazabilidad de resultados.

* `resultados_optimizacion/`
  Archivos asociados al proceso de optimización, candidatos evaluados, resultados de desempeño y selección de alternativas estructurales.

### Lectura recomendada

1. Revisar primero este `README.md`.
2. Abrir la documentación de arquitectura en `docs/`.
3. Revisar los reportes automáticos en `reportes_generados/`.
4. Consultar los resultados de optimización para entender la selección del candidato estructural adoptado.

## Enfoque técnico

* Modelado estructural 3D con OpenSeesPy.
* Análisis modal y respuesta espectral CQC.
* Revisión de derivas, desplazamientos, demanda/capacidad y esbeltez.
* Diseño LRFD de elementos de acero.
* Optimización multiobjetivo mediante NSGA-II.
* Generación automática de reportes en Markdown, CSV y JSON.
* Validación contra software comercial y ejercicios de referencia.

## Caso aplicado

El framework fue aplicado a un edificio metálico de departamentos estudiantiles de cuatro niveles, evaluando alternativas estructurales y seleccionando un sistema con marcos a momento y contravientos Chevron en X.

Resultados principales:

* Peso de acero aproximado: **44.84 t**
* D/C máxima global: **0.793**
* Deriva máxima de servicio: **0.000816**
* Desplazamiento de azotea: **8.35 mm**
* Candidato adoptado: **Rank 55**

Framework en Python para modelado, analisis, validacion normativa, optimizacion genetica y generacion de memorias tecnicas de estructuras metalicas y mixtas.

El proyecto integra:

- OpenSeesPy para el analisis estructural.
- DEAP para optimizacion genetica multiobjetivo NSGA-II.
- Streamlit y Plotly para interfaz visual.
- SQLite para catalogos IMCA de perfiles.
- Excel como fuente externa de datos del proyecto.
- Markdown, CSV y JSON para trazabilidad de resultados.

---

## Estado Actual

El framework ya cuenta con:

- Modelo 3D parametrico desde Excel.
- Lectura de nodos, elementos, cargas, combinaciones y configuracion.
- Convencion documentada de `Orientacion_Beta` para perfiles IR.
- Catalogo IMCA desde SQLite.
- Materiales por familia: IR=A992, OR/OR rellenos=A500 Gr B y A36 como base.
- Espectro de diseno parametrico desde Excel.
- Analisis modal con seleccion automatica de modos por participacion modal minima.
- Respuesta espectral CQC.
- Peso propio automatico de elementos.
- Auditoria de masas, preflight de entrada y constructibilidad.
- Validacion normativa global: D/C, derivas, desplazamiento de azotea, esbeltez y cortante basal.
- Reglas de constructibilidad: espesor minimo, continuidad vertical de columnas, telescopia, ancho en nudos y CFVD segun sistema lateral.
- Optimizador NSGA-II con multiprocessing.
- Evaluacion LRFD detallada dentro de la optimizacion cuando esta activa.
- Exportacion de frente Pareto y candidatos elite con motivos de seleccion.
- Limpieza automatica de corridas antiguas.
- Alias amigables para corridas.
- Interfaz Streamlit.
- Memorias automaticas con evidencia Markdown, JSON y CSV.

Cambios recientes integrados:

- Demanda local gravitacional de vigas: redistribuye cargas nodales tributarias como carga distribuida equivalente por claro fisico.
- Diseno LRFD detallado de vigas IR por flexion, cortante, flexion+cortante y flecha.
- Diseno LRFD detallado de columnas OR simples por compresion, flexion, cortante e interaccion axial-flexion biaxial.
- Diseno LRFD detallado de columnas IR por pandeo flexional y torsional, esbeltez local, flexion fuerte/debil, cortante e interaccion axial-flexion biaxial.
- Diseno axial LRFD de contravientos OR por tension y compresion, incluyendo pandeo global y clasificacion local.
- Exportacion de fuerzas internas LRFD por elemento, combinacion y extremo.
- Envolventes LRFD por elemento y por grupo de diseno.
- Reacciones de base para cimentacion preliminar: detalle por apoyo, resumen por combinacion y envolvente por nodo.
- Memoria de calculo ampliada con deflexiones verticales, fuerzas LRFD, diseno detallado y reacciones de cimentacion.

---

## Abrir el Programa

Forma recomendada:

```powershell
.\run_interfaz_streamlit.ps1
```

Luego abrir:

```text
http://localhost:8501
```

Alternativa directa:

```powershell
.\.venv\Scripts\python.exe -m streamlit run interfaz_streamlit.py --server.port 8501
```

---

## Flujo de Trabajo

1. Preparar el Excel en `auxiliares/1_Nodos_y_Geometria.xlsx` o en la carpeta de insumos elegida.
2. Abrir la interfaz.
3. Revisar modelo 3D, grupos, orientacion beta y espectro.
4. Ejecutar analisis del edificio base.
5. Revisar normativa, derivas, masas, constructibilidad, deflexiones y fuerzas LRFD si aplica.
6. Configurar optimizacion.
7. Lanzar corrida piloto, mediana o grande.
8. Revisar frente Pareto y candidatos elite.
9. Aplicar candidato elite si se desea generar evidencia de esa propuesta.
10. Generar memoria de calculo y archivos CSV/JSON.

Regla operativa: el framework lee el Excel, pero no debe modificarlo sin autorizacion explicita del usuario.

---

## Estructura del Proyecto

```text
.
|-- interfaz_streamlit.py
|-- contexto_proyecto.py
|-- run_interfaz_streamlit.ps1
|-- auxiliares/
|-- core/
|-- data/
|-- config/
|-- analysis/
|   |-- builders/
|   |-- solvers/
|   |-- results/
|   |-- validation/
|   |-- design/
|-- optimization/
|-- herramientas/
|   |-- diagnostico/
|   |-- optimizacion/
|   |-- reportes/
|   |-- legacy/
|-- reportes/
|-- resultados_optimizacion/
|-- reportes_generados/
|-- docs/
```

### Modulos principales

- `contexto_proyecto.py`: construccion comun del proyecto para interfaz, CLI y reportes.
- `interfaz_streamlit.py`: aplicacion visual.
- `auxiliares/`: lectura de Excel.
- `core/`: dominio estructural: nodos, elementos, secciones, materiales y edificio.
- `data/`: catalogos SQLite IMCA.
- `config/`: espectro de diseno.
- `analysis/`: construccion OpenSees, solvers, resultados, validaciones y diseno local.
- `optimization/`: genotipo, constructibilidad, embudo, objetivos y motor genetico.
- `herramientas/`: scripts CLI de diagnostico, optimizacion y reportes.
- `reportes/`: generacion de memoria y evidencia.

---

## Interfaz Streamlit

La interfaz tiene cinco areas principales:

### Modelo

- KPIs del edificio.
- Visualizacion 3D.
- Resumen por tipo y grupo de diseno.
- Orientacion beta.
- Espectro de diseno con fondo blanco, lineas limpias, texto negro y leyenda formal.
- Parametros del espectro leidos desde Excel.

### Analisis

- Ejecuta el motor validado.
- Muestra resumen normativo.
- Muestra constructibilidad.
- Grafica distorsiones.
- Permite auditorias de deflexion vertical y fuerzas internas LRFD.

### Optimizacion

- Configura poblacion, generaciones, nucleos, cruce, mutacion y seed.
- Permite objetivo de deriva automatico o manual.
- Permite alias de corrida.
- Permite forzar columnas OR simples sin editar Excel.
- Lanza el optimizador por subprocess para estabilidad en Windows.

### Resultados

- Lista corridas con nombre tecnico y alias amigable.
- Permite editar alias visible.
- Muestra KPIs del frente Pareto.
- Muestra ranking Pareto, candidatos elite y frente completo.
- Grafica peso, D/C y deriva.
- Permite identificar motivos de seleccion: menor peso, D/C eficiente, deriva equilibrada, menor esbeltez y mejor balance.

### Memoria

- Genera memoria de calculo del edificio actual o del candidato aplicado.
- Exporta Markdown, evidencia JSON y tablas CSV.

---

## Comandos Utiles

Validar entrada:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.diagnostico.validar_entrada --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X
```

Auditar masas:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.diagnostico.auditar_masas --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X
```

Auditar constructibilidad:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.diagnostico.auditar_constructibilidad --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X
```

Auditar deflexiones verticales:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.diagnostico.auditar_deflexiones --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X
```

Exportar fuerzas LRFD:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.diagnostico.exportar_fuerzas_lrfd --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X
```

Corrida parametrica:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.optimizacion.correr_optimizacion_cli --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X --poblacion 72 --generaciones 32 --nucleos 8 --lrfd-detallado rapido --columnas-or-simples
```

Corrida grande usada en el flujo final:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.optimizacion.correr_optimizacion_cli --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X --poblacion 96 --generaciones 48 --nucleos 8 --prob-cruce 0.85 --prob-mutacion 0.35 --objetivo-deriva auto --dc-meta 0.80 --dc-max-diseno 0.95 --factor-dc-sobre-meta 6.0 --lrfd-detallado rapido --columnas-or-simples
```

Generar memoria del edificio del Excel:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.reportes.generar_memoria_cli --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X
```

Generar memoria desde elite:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m herramientas.reportes.generar_memoria_cli --ruta-insumos auxiliares\Pruebas\Proyecto_4_pisos_chevr_X --elite-csv resultados_optimizacion\corrida_x\elite_candidatos.csv --rank 55
```

---

## Optimizacion

El optimizador usa NSGA-II con tres objetivos:

1. Minimizar peso penalizado.
2. Buscar eficiencia demanda/capacidad sin exceder el limite normativo.
3. Equilibrar la utilizacion de deriva para no premiar edificios innecesariamente rigidos.

El motor evalua cada individuo con:

```text
perfil comercial
-> constructibilidad
-> embudo preliminar
-> OpenSeesPy
-> normativa global
-> LRFD detallado si esta activo
-> fitness
```

### Objetivo de deriva automatico

El sistema detecta la presencia de contravientos:

- Sistema arriostrado: `min=0.25`, `meta=0.35`.
- Marco a momento: `min=0.35`, `meta=0.80`.

### LRFD dentro de la optimizacion

Cuando `Validar_LRFD_Detallado_Optimizacion` esta activo, el evaluador revisa:

- Vigas IR: flexion, cortante, flexion+cortante, PLT, pandeo local y flecha.
- Columnas IR: pandeo por ambos ejes y torsional, esbeltez local, flexion fuerte/debil, cortante e interaccion P-M biaxial.
- Columnas OR simples: compresion, flexion, cortante e interaccion P-M biaxial.
- Contravientos OR: tension, compresion, pandeo global y clasificacion local.

El modo `rapido` aprovecha resultados ya calculados en el loop; la exportacion final reevalua candidatos elite con evidencia completa.

### Exportacion

Cada corrida exporta:

- `frente_pareto.csv/json`
- `elite_candidatos.csv/json`
- `resumen.json`

Los candidatos elite incluyen motivos de seleccion:

- menor peso
- D/C mas eficiente
- deriva de servicio mas eficiente
- deriva de colapso mas eficiente
- deriva equilibrada
- menor esbeltez
- mejor balance global
- top por peso

---

## Reportes y Memorias

La memoria automatica incluye:

- alcance
- modelo analizado
- metodologia general
- auditoria de masas
- analisis modal
- resumen normativo
- constructibilidad
- distorsiones de entrepiso
- deflexiones verticales por claros fisicos
- reacciones de base para cimentacion preliminar
- fuerzas internas LRFD por elemento y extremo
- diseno LRFD de vigas IR
- diseno detallado de columnas OR
- diseno axial de contravientos OR
- tablas resumen de elementos criticos
- dictamen automatico

La generacion de memoria produce, segun el caso:

- `*.md`
- `*_evidencia.json`
- `*_columnas.csv`
- `*_deflexiones.csv`
- `*_reacciones_base_detalle.csv`
- `*_reacciones_base_resumen.csv`
- `*_reacciones_base_envolvente.csv`
- `*_reacciones_base.json`
- `*_fuerzas_lrfd_detalle.csv`
- `*_fuerzas_lrfd_envolvente.csv`
- `*_fuerzas_lrfd_envolvente_grupos.csv`
- `*_diseno_vigas_ir_flexion.csv/json`
- `*_diseno_columnas_or.csv/json`
- `*_diseno_contravientos_or.csv/json`

---

## Validaciones Actuales

El framework revisa:

- consistencia de entrada
- masas y peso propio
- participacion modal minima
- espectro y combinaciones
- derivas de servicio
- derivas de colapso
- desplazamiento de azotea
- D/C maximo
- esbeltez de columnas
- cortante basal dinamico vs minimo
- constructibilidad
- demanda/capacidad biaxial para elementos compatibles
- deflexiones verticales de vigas
- fuerzas LRFD y envolventes
- reacciones de base para cimentacion preliminar

La convencion de ejes locales y orientacion de perfiles IR se documenta en [`04_Convencion_Orientacion_Beta.md`](04_Convencion_Orientacion_Beta.md).

---

## Limitaciones y Riesgos Conocidos

El flujo productivo actual es elastico lineal con respuesta espectral.

No estan en produccion todavia:

- pushover
- rotulas plasticas
- modelos de fibras
- tiempo-historia no lineal
- IDA
- degradacion ciclica
- diseno completo de conexiones
- bloque de cortante y area neta en tension de conexiones de contravientos
- diseno LRFD detallado de columnas OR rellenas
- cimentacion formal; solo se exportan reacciones para predimensionamiento

Notas tecnicas importantes:

- Las combinaciones CQC se reportan como envolventes positivas; no conservan un signo concurrente unico.
- Las reacciones de base CQC usan envolvente absoluta de gravedad mas sismo, consistente con el criterio de fuerzas LRFD.
- En vigas IR, `Cb=1.0` y `Lb` se toma entre nodos salvo configuracion explicita; se debe confirmar el arriostramiento lateral real del patin comprimido.
- El factor de resistencia a cortante en los modulos LRFD esta pendiente de parametrizacion normativa; actualmente el codigo conserva `FR_CORTANTE=0.90` en vigas y columnas hasta que se defina el valor final de proyecto.

---

## Roadmap

### Corto plazo

- Parametrizar factores de resistencia LRFD desde configuracion.
- Mejorar memorias por familia de elementos.
- Agregar comparadores automaticos entre corridas.
- Guardar imagenes del modelo y espectro en memoria.

### Mediano plazo

- Diseno de conexiones.
- Modulo auxiliar de cimentacion preliminar.
- Revision LRFD de columnas OR rellenas.
- Analisis pushover para candidatos elite.

### Largo plazo

- Tiempo-historia no lineal.
- Modelos de dano.
- Optimizacion robusta.
- Surrogate models.
- Integracion BIM.

## Estado del proyecto

El framework se encuentra en desarrollo y mejora continua. La versión mostrada en este repositorio corresponde a una etapa documental/showcase, enfocada en demostrar arquitectura, flujo de análisis, validación técnica, optimización estructural y generación automática de reportes.

El código fuente completo no se publica en su totalidad para proteger el desarrollo académico/profesional del proyecto.
