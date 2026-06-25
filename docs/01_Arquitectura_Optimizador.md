# Arquitectura del Framework de Optimizacion Estructural

**Autor:** Abisai  
**Fecha:** Junio 2026  
**Version:** 4.2  
**Estado:** Framework funcional con motor fisico validado, optimizacion multiobjetivo, LRFD detallado, interfaz Streamlit, reacciones de base y memorias trazables.

---

## 1. Vision General

El proyecto es un framework computacional para modelar, analizar, validar y optimizar estructuras metalicas y mixtas. El motor fisico se apoya en OpenSeesPy; el optimizador usa DEAP con NSGA-II; la interfaz operativa se ejecuta en Streamlit.

La idea central es mantener separadas tres responsabilidades:

- El usuario define geometria, cargas, combinaciones, espectro y parametros normativos en Excel.
- El framework traduce esos datos a un modelo estructural interno y lo analiza con OpenSeesPy.
- El optimizador solo decide perfiles comerciales por grupo de diseno; no cambia la topologia ni crea una fisica alterna.

El flujo actual permite trabajar con el edificio introducido en Excel, visualizar el modelo, auditar entrada/masas/constructibilidad, revisar normativa, correr optimizaciones, exportar frente de Pareto, seleccionar candidatos elite y generar memorias con evidencia de diseno.

---

## 2. Principios de Diseno

### Parametrizacion externa

El nucleo no debe contener parametros de proyecto hardcodeados. Geometria, cargas, combinaciones, factores sismicos, limites normativos, participacion modal minima y reglas de optimizacion se inyectan desde Excel o argumentos CLI.

Excepcion: constantes fisicas universales, como la gravedad.

### Motor fisico unico

El analisis de candidatos optimizados se hace con el mismo motor que analiza el edificio base. El optimizador no implementa un analizador alterno:

```text
Excel -> contexto_proyecto -> EdificioBase -> OpenSeesPyInterface -> Evaluador -> OptimizadorGenetico
```

### Trazabilidad

Cada corrida genera una carpeta tecnica:

```text
resultados_optimizacion/corrida_<nombre>
```

Dentro se guardan:

- `frente_pareto.csv`
- `frente_pareto.json`
- `elite_candidatos.csv`
- `elite_candidatos.json`
- `resumen.json`

La interfaz permite alias amigables sin renombrar la carpeta tecnica.

### Conservacion del Excel

El framework lee el Excel, pero no debe modificarlo sin autorizacion explicita del usuario.

---

## 3. Flujo Operativo Actual

### Entrada recomendada

```powershell
.\run_interfaz_streamlit.ps1
```

o directamente:

```powershell
.\.venv\Scripts\python.exe -m streamlit run interfaz_streamlit.py --server.port 8501
```

### Flujo en interfaz

1. **Modelo**
   - Muestra nodos, elementos, peso de acero y masa sismica.
   - Renderiza el modelo 3D con Plotly.
   - Muestra grupos de diseno, perfiles y orientacion beta.
   - Muestra la convencion de `Orientacion_Beta` para columnas IR.
   - Grafica el espectro de diseno leido desde Excel.

2. **Analisis**
   - Ejecuta el analisis del edificio actual.
   - Revisa D/C, derivas, desplazamiento de azotea, esbeltez, cortante basal y constructibilidad.
   - Grafica distorsiones de entrepiso.
   - Permite calcular deflexiones verticales y fuerzas LRFD.

3. **Optimizacion**
   - Configura poblacion, generaciones, nucleos, cruce, mutacion y seed.
   - Permite objetivo de deriva automatico o manual.
   - Permite alias de corrida.
   - Puede forzar columnas OR simples sin editar Excel.
   - Lanza el optimizador como subprocess para estabilidad en Windows con multiprocessing.

4. **Resultados**
   - Lista corridas con nombre amigable.
   - Permite editar alias visible.
   - Muestra resumen de Pareto, candidatos clave y tablas elite.
   - Grafica peso, D/C y utilizacion de deriva.

5. **Memoria**
   - Reanaliza el modelo actual o el candidato aplicado.
   - Genera Markdown, evidencia JSON y tablas CSV.
   - Incluye diseno local LRFD, fuerzas internas, reacciones de base y dictamen automatico.

---

## 4. Topologia por Capas

### `contexto_proyecto.py`

Punto comun de armado del proyecto. Centraliza:

- lectura del Excel
- construccion de materiales por familia de perfil
- carga de catalogo IMCA desde SQLite
- validacion preflight
- construccion del `EdificioBase`
- generacion del espectro
- creacion de `OpenSeesPyInterface`

Esto evita duplicar la creacion del edificio entre interfaz, scripts CLI, optimizador y reportes.

### `interfaz_streamlit.py`

Interfaz visual principal. No reemplaza al motor; lo orquesta:

- carga contexto
- muestra modelo 3D
- muestra espectro y parametros
- ejecuta analisis del edificio actual
- calcula auditorias de masas, deflexiones y fuerzas LRFD
- lanza corridas de optimizacion
- lee resultados exportados
- aplica candidatos elite al modelo en memoria
- genera memorias

### `auxiliares/`

Capa de ingesta de datos:

- `lector_datos.py`: lee nodos, elementos, cargas, configuracion y combinaciones desde Excel.
- `importar_excel.py`: utilidades de importacion.
- `1_Nodos_y_Geometria.xlsx`: archivo maestro si se usa la ruta `auxiliares/`.

### `core/`

Dominio estructural:

- `nodo.py`: coordenadas, restricciones, cargas tributarias y masa sismica.
- `elemento.py`: elemento 3D con nodos, tipo, seccion, longitud, ejes locales y orientacion beta.
- `modelo_edificio.py`: coleccion de nodos, elementos, grupos por tipo y grupos de diseno.
- `secciones.py`: perfiles IR, OR y OR rellenos.
- `materiales.py`: acero y concreto.
- `material_resolver.py`: asigna acero por familia: IR=A992, OR/OR rellenas=A500 Gr B y A36 como respaldo.
- `factories.py`: convierte registros de catalogo en objetos de seccion.
- `constantes_fisicas.py`: constantes fisicas invariantes.

La convencion de orientacion beta se documenta en `docs/04_Convencion_Orientacion_Beta.md`.

### `data/`

Persistencia del catalogo:

- `perfiles_imca.db`: base SQLite de perfiles.
- `gestor_imca.py`: acceso a catalogos IR y OR ordenados.

El optimizador solo elige perfiles existentes en esta base.

### `config/`

Configuracion sismica:

- `generador_espectro.py`: espectro parametrico de diseno.

El espectro usa parametros del Excel, entre ellos `Espectro_a0`, `Espectro_c`, `Espectro_Ta`, `Espectro_Tb`, `Espectro_Tc`, `Factor_Q`, `Factor_R0`, `Factor_Rho`, `Factor_Alfa` y `Amortiguamiento_Xi`.

### `analysis/`

Infraestructura de calculo.

#### Builders

- `builders/model_builder.py`: construccion del modelo OpenSees, diafragma rigido, masas y peso propio automatico.
- `builders/catalog_builder.py`: apoyo a catalogos.

#### Solvers

- `solvers/modal.py`: modos y periodos.
- `solvers/spectral.py`: respuesta modal espectral CQC, participacion modal y fuerzas/reacciones modales.
- `solvers/static.py`: cortante estatico equivalente.
- `solvers/combinations.py`: combinaciones gravitacionales y sismicas; guarda derivas, fuerzas y reacciones de base.

#### Results

- `results/forces.py`: extraccion de fuerzas internas de OpenSees.
- `results/design_forces.py`: fuerzas LRFD por elemento, extremo y combinacion; envolventes por elemento y por grupo.
- `results/base_reactions.py`: reacciones de base para cimentacion preliminar.
- `results/drifts.py`: derivas.
- `results/report.py`: estructuracion de resultados.

#### Validation

- `validation/preflight.py`: validacion de entrada.
- `validation/mass_audit.py`: auditoria de masas.
- `validation/vertical_deflection.py`: auditoria de deflexiones por claros fisicos.

#### Design

- `design/beam_local_gravity.py`: demanda local gravitacional de vigas por carga distribuida equivalente.
- `design/steel_beam_flexure_ntc.py`: diseno LRFD de vigas IR por flexion, cortante, PLT, pandeo local y flecha.
- `design/steel_column_or_ntc.py`: diseno detallado de columnas OR simples.
- `design/steel_column_ir_ntc.py`: diseno detallado de columnas IR con capacidades separadas por eje.
- `design/steel_brace_or_axial_ntc.py`: diseno axial LRFD de contravientos OR.

### `optimization/`

Motor evolutivo:

- `genotipo.py`: genes por grupo de diseno y reparacion de individuos.
- `constructibilidad.py`: reglas de fabricabilidad y compatibilidad.
- `embudo.py`: filtro preliminar por rigidez de planta baja.
- `objetivo_deriva.py`: deteccion del sistema lateral y metas automaticas de deriva.
- `evaluador.py`: aplica perfiles, llama al motor validado y calcula fitness con normativa global y LRFD detallado.
- `motor_ag.py`: NSGA-II, multiprocessing, Pareto, elite y exportacion.
- `ag_optimizador.py`: modulo historico/conservado.

### `herramientas/`

Scripts operativos:

- `diagnostico/validar_entrada.py`
- `diagnostico/auditar_masas.py`
- `diagnostico/auditar_constructibilidad.py`
- `diagnostico/auditar_deflexiones.py`
- `diagnostico/exportar_fuerzas_lrfd.py`
- `optimizacion/correr_optimizacion_cli.py`
- `optimizacion/correr_corrida_grande_mp.py`
- `optimizacion/correr_calibracion_deriva_mp.py`
- `optimizacion/correr_piloto_mediano_mp.py`
- `optimizacion/benchmark_nucleos_optimizador.py`
- `reportes/generar_memoria_cli.py`
- `legacy/`: flujo anterior preservado.

### `reportes/`

Generacion de evidencia:

- `evidencia_diseno.py`: reevalua el modelo y agrupa evidencia.
- `generador_memoria_calculo.py`: memoria tecnica actual.
- `generador_memoria_md.py`: generador historico.

### `resultados_optimizacion/`

Destino de corridas del optimizador. Puede limpiarse automaticamente segun configuracion para evitar saturacion.

### `reportes_generados/`

Destino de memorias, evidencia JSON y CSV.

---

## 5. Consistencia Fisica Blindada

El framework tiene validaciones especificas para evitar inconsistencias fisicas:

- Preflight de entrada antes de construir el modelo.
- Auditoria de masas por nivel, tipo, grupo y combinacion.
- Masa sismica: `CM + CV_INST + masa propia de elementos`.
- Participacion modal minima configurable desde Excel.
- Numero de modos no hardcodeado: se selecciona para alcanzar masa participativa objetivo.
- Espectro parametrico desde Excel.
- Peso propio automatico de elementos.
- Cortante basal dinamico vs minimo.
- Derivas de servicio y colapso.
- Desplazamiento maximo de azotea.
- Esbeltez de columnas.
- Demanda/capacidad biaxial.
- Fuerzas internas LRFD con envolventes.
- Deflexiones verticales de vigas por claros fisicos.
- Reacciones de base para cimentacion preliminar.

### Correccion de vigas con cargas nodales

El modelo global puede tener cargas gravitacionales introducidas en nodos. Para evitar subestimar momentos de vigas, `beam_local_gravity.py` redistribuye las cargas nodales tributarias como una carga distribuida equivalente por claro fisico:

```text
q_equiv = carga_nodal_tributaria / L_claro
Mu = q_equiv L^2 / 8
Vu = q_equiv L / 2
```

El diseno de vigas IR toma:

```text
Mu_control = max(Mu_modelo_global, Mu_local_gravitacional)
Vu_control = max(Vu_modelo_global, Vu_local_gravitacional)
```

---

## 6. Constructibilidad

El optimizador incorpora:

- Espesor minimo de placas/perfiles.
- Misma familia/perfil de columna en altura dentro del stack vertical.
- Telescopia para vigas y contravientos por nivel.
- Compatibilidad de ancho en nudos.
- Contravientos compatibles con columnas.
- CFVD aplicada automaticamente a marcos a momento.
- Penalizaciones y guillotina para descartar soluciones no fabricables.

La regla importante de columnas es que no deben cambiar de perfil en altura dentro de su stack vertical. Si la estructuracion original define grupos diferentes, esos grupos pueden optimizarse por separado.

En sistemas arriostrados, CFVD se omite por defecto porque el mecanismo lateral principal depende de contravientos. Si se requiere forzar esa revision en arriostrados, puede activarse explicitamente con `CFVD_Aplicar_En_Arriostrados`.

---

## 7. Optimizacion

El optimizador usa NSGA-II con tres objetivos:

1. Minimizar peso penalizado.
2. Maximizar eficiencia demanda/capacidad sin exceder norma.
3. Equilibrar la utilizacion de deriva sin premiar rigidez innecesaria.

La evaluacion de cada individuo sigue:

```text
genotipo
-> perfiles comerciales
-> constructibilidad
-> embudo
-> OpenSees
-> metricas normativas
-> LRFD detallado si esta activo
-> fitness
```

### Multiprocessing

En Windows se usa `spawn`; por estabilidad, la interfaz lanza la optimizacion como subprocess:

```text
herramientas.optimizacion.correr_optimizacion_cli
```

### Objetivo de deriva automatico

`optimization/objetivo_deriva.py` detecta el sistema lateral:

- Con contravientos: `arriostrado`, con `min=0.25`, `meta=0.35`.
- Sin contravientos: `marco_momento`, con `min=0.35`, `meta=0.80`.

El usuario puede forzar valores manuales desde interfaz o CLI.

### LRFD en optimizacion

`optimization/evaluador.py` puede activar validacion LRFD detallada:

La validacion incluye vigas IR, columnas IR, columnas OR simples y contravientos OR.

- `rapido`: usa fuerzas derivadas de resultados ya calculados en el evaluador.
- `completo`: reextrae fuerzas LRFD desde combinaciones completas.
- `apagado`: no penaliza por LRFD detallado.

La exportacion de candidatos elite reevalua con evidencia completa cuando `LRFD_Detallado_Exportacion_Completa` esta activo.

### Exportacion

Cada corrida exporta:

- frente completo
- candidatos elite
- motivos de seleccion
- resumen normativo
- alias y nombre amigable
- parametros de objetivo de deriva

Los motivos de seleccion incluyen:

- menor peso
- D/C mas eficiente
- deriva de servicio mas eficiente
- deriva de colapso mas eficiente
- deriva equilibrada
- menor esbeltez
- mejor balance global
- top por peso

---

## 8. Reportes y Memorias

La memoria generada incluye:

- metodologia
- modelo analizado
- auditoria de masas
- analisis modal
- resumen normativo
- constructibilidad
- derivas
- deflexiones verticales
- reacciones de base
- fuerzas internas LRFD
- diseno LRFD de vigas IR
- diseno detallado de columnas OR
- diseno axial de contravientos OR
- tablas resumen
- dictamen

### Reacciones de base

`analysis/results/base_reactions.py` extrae:

- detalle por nodo y combinacion
- resumen por combinacion
- envolvente por nodo de base

Convencion:

- `R*` es la reaccion del apoyo sobre la estructura.
- `*_cimentacion` es la accion opuesta transmitida a la cimentacion.
- En gravedad se conserva signo.
- En CQC se usa envolvente absoluta de gravedad mas sismo.
- El momento de resumen se calcula respecto al centro geometrico de apoyos de base.

---

## 9. Limitaciones Actuales

El flujo principal esta validado para analisis elastico lineal con respuesta modal espectral.

Todavia no forman parte del flujo productivo:

- pushover
- rotulas plasticas
- modelos de fibras
- degradacion ciclica
- histeresis avanzada
- tiempo-historia no lineal
- IDA
- diseno completo de conexiones
- diseno LRFD detallado de columnas OR rellenas
- cimentacion formal; solo se exportan reacciones para predimensionamiento

Riesgos tecnicos documentados:

- Las combinaciones CQC son envolventes y no conservan signos concurrentes.
- En vigas IR, `Cb=1.0` y `Lb` se toma entre nodos salvo configuracion explicita; se debe verificar arriostramiento lateral real.
- El factor de resistencia a cortante `FR_CORTANTE` sigue pendiente de parametrizacion normativa; actualmente el codigo conserva `0.90` hasta definir el valor final del proyecto.
- Contravientos OR no revisan conexiones, bloque de cortante ni area neta.

---

## 10. Hoja de Ruta

### Corto plazo

- Parametrizar factores LRFD desde configuracion.
- Mejorar memoria por familia de elementos.
- Agregar tablas comparativas entre corridas.
- Guardar snapshots visuales del modelo y espectro.
- Afinar calibraciones por sistema lateral.

### Mediano plazo

- Diseno de conexiones.
- Modulo auxiliar de cimentacion preliminar.
- Revision LRFD de columnas OR rellenas.
- Analisis pushover para candidatos elite.
- Comparacion automatica entre alternativas del frente Pareto.

### Largo plazo

- Tiempo-historia no lineal.
- Modelos de dano.
- Optimizacion robusta ante incertidumbre.
- Surrogate models.
- Integracion BIM.

---

## 11. Estado de Validacion

El framework ya cuenta con blindajes de consistencia fisica en:

- entrada de datos
- masas
- espectro
- modos
- combinaciones
- derivas
- cortante basal
- constructibilidad
- demanda/capacidad biaxial
- deflexiones verticales
- fuerzas LRFD
- LRFD detallado de vigas IR, columnas OR simples y contravientos OR
- reacciones de base
- exportacion de resultados

El optimizador esta conectado al motor validado, lo que evita que cree su propia logica fisica. La siguiente etapa de validacion debe concentrarse en parametrizar factores LRFD, conexiones, cimentacion preliminar formal y analisis avanzado de candidatos elite.
