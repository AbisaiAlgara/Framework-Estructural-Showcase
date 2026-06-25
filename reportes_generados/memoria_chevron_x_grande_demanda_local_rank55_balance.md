# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-06-09 19:43:38

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 132 |
| Elementos | 280 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 44,835.37 |
| Masa sismica (kg) | 613,157.10 |

## 3. Metodologia General

El modelo se ensambla en OpenSees a partir de nodos, elementos, grupos de diseno, perfiles comerciales y orientacion beta introducidos en el archivo de entrada. Las combinaciones se evalúan con gravedad y respuesta modal espectral CQC cuando corresponde.

La masa sismica se calcula como:

`M_s = CM + CV_INST + M_propia_elementos`

La deriva inelastica de colapso se calcula con:

`delta_colapso = delta_elastica * Q * R0 * rho / alfa`

La aceptacion global requiere que las demandas normalizadas sean menores o iguales a 1.0.

## 4. Auditoria de Masas

La auditoria separa cargas nodales, masa propia de elementos y masa sismica efectiva.

| Parametro | Valor |
| --- | --- |
| CM total (kgf) | 491,816.88 |
| CV instantanea total (kgf) | 76,504.85 |
| Masa propia elementos (kg) | 44,835.37 |
| Masa sismica total (kg) | 613,157.10 |
| Peso sismico total (N) | 6,015,071.12 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 2,472.83 | 2,472.83 |
| 3.2000 | 122,954.22 | 19,126.21 | 11,565.64 | 153,646.07 |
| 6.4000 | 122,954.22 | 19,126.21 | 11,208.84 | 153,289.27 |
| 9.6000 | 122,954.22 | 19,126.21 | 10,852.04 | 152,932.48 |
| 12.8000 | 122,954.22 | 19,126.21 | 8,736.01 | 150,816.44 |

## 5. Analisis Modal

El numero de modos se selecciona buscando alcanzar la participacion modal minima configurada.

| Parametro | Valor |
| --- | --- |
| Objetivo participacion | 0.9 |
| Modos usados | 5 |
| Maximo modos |  |

### 5.1 Periodos

| Modo | Periodo (s) |
| --- | --- |
| 1 | 0.405264 |
| 2 | 0.401759 |
| 3 | 0.25508 |
| 4 | 0.139998 |
| 5 | 0.139188 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.773616 |
| 2 |  |  | 0.77396 |
| 3 |  |  | 0.774022 |
| 4 |  |  | 0.955769 |
| 5 |  |  | 0.955807 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.000337227 |
| 2 |  |  | 0.770677 |
| 3 |  |  | 0.771679 |
| 4 |  |  | 0.771721 |
| 5 |  |  | 0.95438 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.793307 | 1.0000 | Cumple |
| Deriva servicio | 0.000816394 | 0.005 | Cumple |
| Deriva colapso | 0.00362842 | 0.01 | Cumple |
| Desp. azotea | 0.00834632 | 0.1 | Cumple |
| KL/r | 46.8008 | 200.0000 | Cumple |
| Cortante basal | 1.0000 | >= 1.0 si requiere escala | Cumple |

## 7. Criterios Constructivos

La revision constructiva penaliza configuraciones no fabricables o incompatibles con los criterios definidos para el proyecto.

| Criterio | Penalizacion | Estado |
| --- | --- | --- |
| Espesor minimo | 0 | Cumple |
| Mismo perfil de columna en altura | 0 | Cumple |
| Telescopia | 0 | Cumple |
| Ancho en nudos | 0 | Cumple |
| CFVD | 0 | Cumple |

## 8. Distorsiones de Entrepiso

Se reporta la envolvente de distorsiones por nivel. La deriva de colapso se obtiene multiplicando la deriva elastica por el factor inelastico configurado.

| Nivel | Combo critico | Deriva servicio | Deriva colapso |
| --- | --- | --- | --- |
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000444988 | 0.00197773 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.00068177 | 0.00303009 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.000816394 | 0.00362842 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.000665073 | 0.00295588 |

### 8.4 Revision de Deflexiones Verticales

La revision se realiza bajo combinacion gravitacional de servicio. Los elementos de viga colineales se agrupan en claros fisicos para no evaluar tramos de mallado como vigas independientes.

La flecha local se calcula respecto a la cuerda deformada entre apoyos:

`delta_local(x) = Uz(x) - Uz_cuerda(x)`

El criterio de aceptacion es:

`delta_max <= L/240`

| Parametro | Valor |
| --- | --- |
| Combinacion | GRAV_SERVICIO |
| Metodo | Flecha local nodal contra cuerda entre apoyos del claro fisico |
| Claros revisados | 104 |
| Claros que cumplen | 104 |
| Claros en revision | 0 |
| Ratio max | 0.0919557 |
| Flecha maxima (mm) | 2.0307 |
| Flecha admisible (mm) | 22.0833 |
| Estado global | Cumple |

#### Claros Criticos

| Claro | Nivel (m) | Grupo | Perfil | Elementos | L (m) | delta max (mm) | delta adm (mm) | Ratio | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DV-0024 | 12.8000 | Viga_2 | IR 356 x 44.6 | 430, 431 | 5.3000 | 2.0307 | 22.0833 | 0.0919557 | Cumple |
| DV-0036 | 3.2000 | Viga_1 | IR 356 x 44.6 | 149, 150 | 6.1000 | 2.3063 | 25.4167 | 0.0907379 | Cumple |
| DV-0010 | 12.8000 | Viga_2 | IR 356 x 44.6 | 449, 450 | 6.1000 | 2.3019 | 25.4167 | 0.0905664 | Cumple |
| DV-0062 | 6.4000 | Viga_1 | IR 356 x 44.6 | 249, 250 | 6.1000 | 2.2942 | 25.4167 | 0.0902625 | Cumple |
| DV-0088 | 9.6000 | Viga_1 | IR 356 x 44.6 | 349, 350 | 6.1000 | 2.2890 | 25.4167 | 0.090058 | Cumple |
| DV-0018 | 12.8000 | Viga_2 | IR 356 x 44.6 | 437, 438 | 5.3000 | 1.7391 | 22.0833 | 0.0787504 | Cumple |
| DV-0050 | 3.2000 | Viga_1 | IR 356 x 44.6 | 130, 131 | 5.3000 | 1.7273 | 22.0833 | 0.0782154 | Cumple |
| DV-0076 | 6.4000 | Viga_1 | IR 356 x 44.6 | 230, 231 | 5.3000 | 1.6663 | 22.0833 | 0.075456 | Cumple |
| DV-0102 | 9.6000 | Viga_1 | IR 356 x 44.6 | 330, 331 | 5.3000 | 1.6085 | 22.0833 | 0.0728362 | Cumple |
| DV-0044 | 3.2000 | Viga_1 | IR 356 x 44.6 | 137, 138 | 5.3000 | 1.4698 | 22.0833 | 0.0665551 | Cumple |
| DV-0070 | 6.4000 | Viga_1 | IR 356 x 44.6 | 237, 238 | 5.3000 | 1.4300 | 22.0833 | 0.0647525 | Cumple |
| DV-0096 | 9.6000 | Viga_1 | IR 356 x 44.6 | 337, 338 | 5.3000 | 1.3896 | 22.0833 | 0.0629264 | Cumple |

#### Alertas

- 56 claros no tienen nodos intermedios; la flecha local puede estar subestimada.

Nota: en claros sin nodos intermedios el indicador nodal puede subestimar la flecha maxima local; esos casos deben refinarse si controlan la revision.

### 8.5 Reacciones de Base para Cimentacion Preliminar

Se reportan las reacciones en los nodos empotrados del nivel base para apoyar el predimensionamiento de cimentacion. Las componentes `R` corresponden a la reaccion del apoyo sobre la estructura; la accion transmitida a la cimentacion tiene signo opuesto.

En combinaciones gravitacionales las reacciones se conservan con signo. En combinaciones espectrales CQC se reporta una envolvente absoluta de gravedad mas sismo, consistente con el criterio usado para fuerzas internas LRFD.

#### Resumen por Combinacion

| Combo | Criterio | Nodos | P total (kN) | Vh (kN) | Mx (kN-m) | My (kN-m) | M vol. (kN-m) | P max nodo (kN) | Uplift total (kN) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SISMO_DOWN_1 | envolvente_abs_grav_cqc | 16 | 8,636.69 | 1,136.75 | -859.9176 | 5,258.41 | 5,328.26 | 819.7180 | 0 |
| SISMO_DOWN_2 | envolvente_abs_grav_cqc | 16 | 8,636.69 | 1,136.75 | -859.9176 | 5,258.41 | 5,328.26 | 819.7180 | 0 |
| SISMO_DOWN_3 | envolvente_abs_grav_cqc | 16 | 8,636.69 | 1,136.75 | -859.9176 | 5,258.41 | 5,328.26 | 819.7180 | 0 |
| SISMO_DOWN_4 | envolvente_abs_grav_cqc | 16 | 8,636.69 | 1,136.75 | -859.9176 | 5,258.41 | 5,328.26 | 819.7180 | 0 |
| GRAV_DISEÑO | gravedad_signada | 16 | 8,452.17 | 9.63565e-11 | -965.6757 | 6,605.16 | 6,675.38 | 1,041.47 | 0 |
| SISMO_DOWN_5 | envolvente_abs_grav_cqc | 16 | 7,224.58 | 657.8259 | -774.2172 | 5,193.78 | 5,251.16 | 815.8624 | 0 |
| SISMO_DOWN_6 | envolvente_abs_grav_cqc | 16 | 7,224.58 | 657.8259 | -774.2172 | 5,193.78 | 5,251.16 | 815.8624 | 0 |
| SISMO_DOWN_7 | envolvente_abs_grav_cqc | 16 | 7,224.58 | 657.8259 | -774.2172 | 5,193.78 | 5,251.16 | 815.8624 | 0 |
| SISMO_DOWN_8 | envolvente_abs_grav_cqc | 16 | 7,224.58 | 657.8259 | -774.2172 | 5,193.78 | 5,251.16 | 815.8624 | 0 |
| SISMO_UP_1 | envolvente_abs_grav_cqc | 16 | 6,758.21 | 1,013.88 | -649.0209 | 3,758.91 | 3,814.53 | 587.0827 | 0 |
| SISMO_UP_2 | envolvente_abs_grav_cqc | 16 | 6,758.21 | 1,013.88 | -649.0209 | 3,758.91 | 3,814.53 | 587.0827 | 0 |
| SISMO_UP_3 | envolvente_abs_grav_cqc | 16 | 6,758.21 | 1,013.88 | -649.0209 | 3,758.91 | 3,814.53 | 587.0827 | 0 |

#### Envolvente por Nodo de Base

| Nodo | X (m) | Y (m) | P max (kN) | Combo P | Uplift (kN) | Vh max (kN) | Combo V | M nodo (kN-m) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 6 | 6.3200 | 6.2000 | 1,041.47 | GRAV_DISEÑO | 0 | 15.0434 | SISMO_DOWN_1 | 22.9274 |
| 10 | 6.3200 | 12.3000 | 992.4612 | GRAV_DISEÑO | 0 | 13.5735 | SISMO_DOWN_1 | 21.3282 |
| 7 | 11.6200 | 6.2000 | 779.0874 | GRAV_DISEÑO | 0 | 12.7141 | SISMO_DOWN_1 | 20.0053 |
| 2 | 6.3200 | 0 | 635.7480 | SISMO_DOWN_1 | 0 | 135.8403 | SISMO_DOWN_1 | 4.1603 |
| 14 | 6.3200 | 18.5000 | 627.0853 | SISMO_DOWN_1 | 0 | 134.2740 | SISMO_DOWN_1 | 4.0984 |
| 15 | 11.6200 | 18.5000 | 588.6238 | SISMO_DOWN_1 | 0 | 120.9011 | SISMO_DOWN_1 | 4.2168 |
| 3 | 11.6200 | 0 | 577.5667 | SISMO_DOWN_1 | 0 | 120.3491 | SISMO_DOWN_1 | 4.3250 |
| 13 | 0 | 18.5000 | 552.0333 | SISMO_DOWN_1 | 0 | 141.0012 | SISMO_DOWN_1 | 3.4397 |
| 5 | 0 | 6.2000 | 534.7047 | GRAV_DISEÑO | 0 | 59.6669 | SISMO_DOWN_1 | 3.9926 |
| 1 | 0 | 0 | 533.4117 | SISMO_DOWN_1 | 0 | 139.7745 | SISMO_DOWN_1 | 3.5034 |
| 11 | 11.6200 | 12.3000 | 532.9307 | GRAV_DISEÑO | 0 | 10.7431 | SISMO_DOWN_1 | 18.4231 |
| 9 | 0 | 12.3000 | 520.5483 | GRAV_DISEÑO | 0 | 56.9042 | SISMO_DOWN_1 | 3.8968 |
| 16 | 17.0700 | 18.5000 | 491.1021 | SISMO_DOWN_1 | 0 | 123.6249 | SISMO_DOWN_1 | 3.4705 |
| 4 | 17.0700 | 0 | 486.7576 | SISMO_DOWN_1 | 0 | 124.5702 | SISMO_DOWN_1 | 3.5509 |
| 8 | 17.0700 | 6.2000 | 388.6288 | GRAV_DISEÑO | 0 | 47.2525 | SISMO_DOWN_1 | 4.1378 |
| 12 | 17.0700 | 12.3000 | 247.2109 | GRAV_DISEÑO | 0 | 38.6613 | SISMO_DOWN_1 | 3.7483 |

#### Alertas

_Sin alertas._

### 8.6 Fuerzas Internas para Diseno LRFD

Se exportan fuerzas locales por elemento, combinacion y extremo para uso en revisiones LRFD. Las combinaciones de gravedad se reportan con signo local. En combinaciones sismicas por CQC se reporta una envolvente positiva de diseno, ya que el metodo modal espectral no conserva un signo concurrente unico.

| Parametro | Valor |
| --- | --- |
| Elementos | 280 |
| Combinaciones | 18 |
| Filas detalle | 10080 |
| Filas envolvente | 280 |
| Filas envolvente grupos | 6 |
| Incluye CQC | Si |
| Elemento momento critico | 330 |
| M max (kN-m) | 63.8871 |
| Combo momento critico | GRAV_DISEÑO |
| Elemento axial critico | 106 |
| P max (kN) | 1,041.47 |
| Combo axial critico | GRAV_DISEÑO |
| Grupo momento critico | Viga_1 |
| Grupo M max (kN-m) | 63.8871 |
| Grupo axial critico | Columna_2 |
| Grupo P max (kN) | 1,041.47 |

#### Envolvente por Grupo de Diseno

| Grupo | Tipo | Perfiles | Elems | P max (kN) | Elem P | Combo P | V max (kN) | Elem V | Combo V | M max (kN-m) | Elem M | Combo M |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Columna_1 | COLUMNA | ORC 178 x 9.5 | 48 | 503.4713 | 102 | SISMO_DOWN_1 | 4.7355 | 408 | SISMO_DOWN_1 | 8.4030 | 408 | SISMO_DOWN_1 |
| Columna_2 | COLUMNA | ORC 254 x 15.9 | 16 | 1,041.47 | 106 | GRAV_DISEÑO | 23.8162 | 406 | SISMO_DOWN_1 | 43.2721 | 406 | SISMO_DOWN_1 |
| Contraviento_1 | CONTRAVIENTO | ORC 127 x 6.4 | 32 | 133.7165 | 156 | SISMO_DOWN_1 | 140.2530 | 257 | SISMO_DOWN_1 | 0 | 0 |  |
| Contraviento_2 | CONTRAVIENTO | ORC 76 x 6.4 | 32 | 77.4363 | 367 | SISMO_DOWN_1 | 83.3059 | 358 | SISMO_DOWN_1 | 0 | 0 |  |
| Viga_1 | VIGA | IR 356 x 44.6 | 114 | 4.16321e-14 | 352 | SISMO_DOWN_5 | 75.3351 | 130 | GRAV_DISEÑO | 63.8871 | 330 | GRAV_DISEÑO |
| Viga_2 | VIGA | IR 356 x 44.6 | 38 | 8.25546e-14 | 452 | GRAV_SERVICIO | 73.8276 | 430 | GRAV_DISEÑO | 56.4031 | 430 | GRAV_DISEÑO |

#### Envolvente Critica por Momento Resultante

| Elem | Tipo | Grupo | Perfil | M max (kN-m) | Combo M | Extremo M | P max (kN) | Combo P | V max (kN) | Combo V |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 330 | VIGA | Viga_1 | IR 356 x 44.6 | 63.8871 | GRAV_DISEÑO | I | 0 |  | 74.9660 | GRAV_DISEÑO |
| 230 | VIGA | Viga_1 | IR 356 x 44.6 | 63.5568 | GRAV_DISEÑO | I | 0 |  | 75.1294 | GRAV_DISEÑO |
| 130 | VIGA | Viga_1 | IR 356 x 44.6 | 63.2273 | GRAV_DISEÑO | I | 0 |  | 75.3351 | GRAV_DISEÑO |
| 430 | VIGA | Viga_2 | IR 356 x 44.6 | 56.4031 | GRAV_DISEÑO | I | 0 |  | 73.8276 | GRAV_DISEÑO |
| 431 | VIGA | Viga_2 | IR 356 x 44.6 | 53.6809 | GRAV_DISEÑO | I | 0 |  | 23.9473 | GRAV_DISEÑO |
| 237 | VIGA | Viga_1 | IR 356 x 44.6 | 53.6615 | SISMO_DOWN_1 | I | 0 |  | 62.8209 | GRAV_DISEÑO |
| 337 | VIGA | Viga_1 | IR 356 x 44.6 | 53.3500 | SISMO_DOWN_1 | I | 0 |  | 62.4741 | GRAV_DISEÑO |
| 137 | VIGA | Viga_1 | IR 356 x 44.6 | 52.3866 | GRAV_DISEÑO | I | 0 |  | 63.2332 | GRAV_DISEÑO |
| 131 | VIGA | Viga_1 | IR 356 x 44.6 | 49.1182 | GRAV_DISEÑO | I | 0 |  | 22.4839 | GRAV_DISEÑO |
| 231 | VIGA | Viga_1 | IR 356 x 44.6 | 48.4804 | GRAV_DISEÑO | I | 0 |  | 22.6202 | SISMO_DOWN_1 |
| 331 | VIGA | Viga_1 | IR 356 x 44.6 | 47.9051 | GRAV_DISEÑO | I | 0 |  | 22.6913 | GRAV_DISEÑO |
| 352 | VIGA | Viga_1 | IR 356 x 44.6 | 47.6855 | GRAV_DISEÑO | I | 4.16321e-14 | SISMO_DOWN_5 | 32.2190 | GRAV_DISEÑO |

#### Alertas

- SISMO_DOWN_1: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_2: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_3: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_4: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_5: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_6: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_7: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_DOWN_8: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_1: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_2: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_3: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_4: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_5: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_6: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_7: sismo CQC reportado como envolvente positiva de diseno.
- SISMO_UP_8: sismo CQC reportado como envolvente positiva de diseno.

### 8.7 Diseno LRFD por Flexion de Vigas IR

La revision toma la mayor demanda entre la envolvente LRFD del modelo global y una revision gravitacional local por claro fisico. La demanda local redistribuye las cargas nodales tributarias como una carga distribuida equivalente sobre la viga, evitando que los tramos largos sin nodos intermedios queden subestimados en flexion o flecha. Para vigas IR se revisa la flexion alrededor del eje mayor `x-x` IMCA. Se usa `Cb = 1.0` y `Lb` igual a la longitud entre nodos del elemento, salvo configuracion explicita.

#### Resumen General

| Parametro | Valor |
| --- | --- |
| Vigas IR revisadas | 152 |
| Cumplen | 152 |
| No cumplen | 0 |
| Vigas controladas por demanda local | 144 |
| Elemento critico | 132 |
| Grupo critico | Viga_1 |
| Perfil critico | IR 356 x 44.6 |
| Combo critico | GRAV_DISEÑO |
| Demanda control | local_gravitacional |
| Mu max (kN-m) | 68.7481 |
| Mu modelo (kN-m) | 2.3885 |
| Mu local (kN-m) | 68.7481 |
| MR control (kN-m) | 93.5829 |
| Utilizacion flexion max | 0.734622 |
| Utilizacion control max | 0.744615 |
| Estado limite control | PLT |

#### Calculo Detallado de la Viga Critica

| Paso / Propiedad | Valor |
| --- | --- |
| 1. Perfil | IR 356 x 44.6 |
| 2. Eje de flexion | x-x IMCA / eje fuerte |
| Elemento | 132 |
| Grupo | Viga_1 |
| Combinacion Mu | GRAV_DISEÑO |
| Extremo Mu | VL-0034 |
| Demanda control | local_gravitacional |
| Mu modelo (kN-m) | 2.3885 |
| Mu local (kN-m) | 68.7481 |
| Claro local | VL-0034 |
| Longitud claro local (m) | 6.1000 |
| w diseno local (kN/m) | 14.7806 |
| Material | A992 |
| Fy (kg/cm2) | 3,515.00 |
| E (kg/cm2) | 2,039,696.53 |
| d (mm) | 351.0000 |
| bf (mm) | 171.0000 |
| tf (mm) | 9.8000 |
| tw (mm) | 6.9000 |
| h (mm) | 311.0000 |
| Ix (cm4) | 12,113.00 |
| Iy (cm4) | 816.0000 |
| Sx (cm3) | 689.0000 |
| Zx (cm3) | 776.0000 |
| ry (cm) | 3.8000 |
| J (cm4) | 16.0000 |
| Cw (cm6) | 238,191.00 |
| lambda_f = bf / 2tf | 8.7245 |
| lambda_f limite compacto | 9.1523 |
| lambda_f limite no compacto | 24.0850 |
| Clasificacion patin | compacta |
| lambda_w = h / tw | 45.0725 |
| lambda_w limite compacto | 59.0082 |
| lambda_w limite no compacto | 134.8758 |
| Clasificacion alma | compacta |
| 6. Inciso NTC seleccionado | 7.3 |
| Lb (m) | 6.1000 |
| Cb | 1.0000 |
| Lu (m) | 1.6108 |
| Lr (m) | 4.5343 |
| Mu (kN-m) | 68.7481 |
| Vu (kN) | 45.0807 |

#### Estados Limite de Flexion

| Estado | Nombre | Inciso | Aplica | Mn (kN-m) | MR (kN-m) | Util | Resultado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| F | Fluencia | NTC-Acero 7.3.1 / 7.4.2 | Si | 267.5815 | 240.8233 | 0.285471 | Cumple |
| PLT | Pandeo lateral por flexotorsion | NTC-Acero 7.3.2 / 7.4.3 | Si | 103.9810 | 93.5829 | 0.734622 | Cumple |
| PLP | Pandeo local del patin comprimido | NTC-Acero 7.4.4 | No | 267.5815 | 240.8233 | 0.285471 | Cumple |
| PLA | Pandeo local del alma por flexion | NTC-Acero 7.5 | No | 267.5815 | 240.8233 | 0.285471 | Cumple |

#### Verificacion Resistente

| Revision | Valor |
| --- | --- |
| 8. Mn control (kN-m) | 103.9810 |
| Estado limite control | PLT |
| 9. MR = FR Mn (kN-m) | 93.5829 |
| Mu modelo (kN-m) | 2.3885 |
| Mu local (kN-m) | 68.7481 |
| 10. Mu / MR | 0.734622 |
| Vn (kN) | 501.0739 |
| 11. VR (kN) | 450.9665 |
| Vu / VR | 0.0999647 |
| 12. Mu/MR + (Vu/VR)^2 | 0.744615 |
| Estado final | Cumple |

#### Referencia de Servicio

| Parametro | Valor |
| --- | --- |
| 13. Claro de deflexion asociado | VL-0034 |
| Ratio flecha | 0.324142 |
| Delta max (mm) | 8.2386 |
| Delta adm (mm) | 25.4167 |
| Estado flecha | Cumple |
| Flecha local calculada (mm) | 8.2386 |
| Flecha local admisible (mm) | 25.4167 |
| Ratio flecha local | 0.324142 |

#### Resumen de Diseno de Vigas IR

| Elem | Grupo | Perfil | Inciso | Demanda | Mu (kN-m) | Mu modelo | Mu local | MR (kN-m) | Vu (kN) | VR (kN) | Flexion | Cortante | F+V | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 132 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 68.7481 | 2.3885 | 68.7481 | 93.5829 | 45.0807 | 450.9665 | 0.734622 | 0.0999647 | 0.744615 | Cumple |
| 232 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 68.7481 | 2.8319 | 68.7481 | 93.5829 | 45.0807 | 450.9665 | 0.734622 | 0.0999647 | 0.744615 | Cumple |
| 332 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 68.7481 | 3.1725 | 68.7481 | 93.5829 | 45.0807 | 450.9665 | 0.734622 | 0.0999647 | 0.744615 | Cumple |
| 432 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 68.7481 | 3.0658 | 68.7481 | 93.5829 | 45.0807 | 450.9665 | 0.734622 | 0.0999647 | 0.744615 | Cumple |
| 134 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 15.7259 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 234 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 16.3175 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 334 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 15.6664 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 434 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 15.4894 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 128 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 17.0368 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 228 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 17.5662 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 328 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 16.8112 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 428 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 64.7580 | 17.6718 | 64.7580 | 88.7482 | 40.9861 | 450.9665 | 0.729682 | 0.0908849 | 0.737942 | Cumple |
| 122 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 3.2185 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 222 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 4.3684 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 322 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 4.9357 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 422 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 4.3647 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 139 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 2.8452 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 239 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 3.6344 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 339 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 3.9571 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 439 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 61.4876 | 3.7072 | 61.4876 | 91.3251 | 39.6694 | 450.9665 | 0.673282 | 0.0879653 | 0.68102 | Cumple |
| 129 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 56.0686 | 1.2671 | 56.0686 | 93.5829 | 36.7663 | 450.9665 | 0.599132 | 0.0815277 | 0.605779 | Cumple |
| 229 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 56.0686 | 2.1072 | 56.0686 | 93.5829 | 36.7663 | 450.9665 | 0.599132 | 0.0815277 | 0.605779 | Cumple |
| 329 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 56.0686 | 1.5220 | 56.0686 | 93.5829 | 36.7663 | 450.9665 | 0.599132 | 0.0815277 | 0.605779 | Cumple |
| 429 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 56.0686 | 1.9889 | 56.0686 | 93.5829 | 36.7663 | 450.9665 | 0.599132 | 0.0815277 | 0.605779 | Cumple |
| 125 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 50.8860 | 9.8314 | 50.8860 | 91.3251 | 32.8297 | 450.9665 | 0.557196 | 0.0727984 | 0.562495 | Cumple |
| 225 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 50.8860 | 9.4127 | 50.8860 | 91.3251 | 32.8297 | 450.9665 | 0.557196 | 0.0727984 | 0.562495 | Cumple |
| 325 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 50.8860 | 9.2126 | 50.8860 | 91.3251 | 32.8297 | 450.9665 | 0.557196 | 0.0727984 | 0.562495 | Cumple |
| 425 | Viga_2 | IR 356 x 44.6 | 7.3 | local_gravitacional | 50.8860 | 15.4883 | 50.8860 | 91.3251 | 32.8297 | 450.9665 | 0.557196 | 0.0727984 | 0.562495 | Cumple |
| 141 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 46.0402 | 3.2518 | 46.0402 | 91.3251 | 29.7033 | 450.9665 | 0.504135 | 0.0658659 | 0.508473 | Cumple |
| 241 | Viga_1 | IR 356 x 44.6 | 7.3 | local_gravitacional | 46.0402 | 4.4912 | 46.0402 | 91.3251 | 29.7033 | 450.9665 | 0.504135 | 0.0658659 | 0.508473 | Cumple |

#### Alertas

Demanda controlada por revision gravitacional local de claro fisico.

- PLT controla al menos una viga IR con Cb=1.0 y Lb entre nodos; confirmar arriostramiento lateral real del patin comprimido antes de dictaminar el diseno final.
- 56 claros sin nodos intermedios: la revision local usa formulas cerradas de viga simplemente apoyada para capturar momento y flecha maxima.

## 9. Diseno de Columnas

### 9.1 Metodologia

Para cada columna se obtiene la demanda axial `Pu` y los momentos locales separados `My` y `Mz` por combinacion. La resistencia axial se evalua con `phi Pn` considerando esbeltez, y la flexion se revisa con capacidades por eje: `phi Mny` y `phi Mnz`.

El radio de giro minimo se calcula como:

`r_min = sqrt(min(Ix, Iy) / A)`

La esbeltez se calcula como:

`KL/r = K L / r_min`

La interaccion flexocompresion se evalua con:

`Si Pu/phiPn >= 0.20: D/C = Pu/phiPn + 8/9 (My/phiMny + Mz/phiMnz)`

`Si Pu/phiPn < 0.20: D/C = Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`

El criterio de aceptacion es `D/C <= 1.0`.

### 9.2 Columna Critica Desarrollada

| Dato | Valor |
| --- | --- |
| Elemento | 406 |
| Grupo | Columna_2 |
| Perfil | ORC 254 x 15.9 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.013548 |
| Ix (m4) | 0.000126534 |
| Iy (m4) | 0.000126534 |
| r_min (m) | 0.096642 |
| KL/r | 33.1119 |
| Pu (kN) | 23.7447 |
| Mu (kN-m) | 43.1431 |
| My local (kN-m) | 43.1431 |
| Mz local (kN-m) | 0.0446059 |
| phi Pn (kN) | 3,594.38 |
| phi Mny (kN-m) | 342.5990 |
| phi Mnz (kN-m) | 342.5990 |
| Extremo critico | j |
| Tipo revision | OR biaxial simetrico: misma capacidad en ambos ejes locales |

Sustitucion numerica:

- `Pu/phiPn = 23.745 / 3594.377 = 0.0066`
- `My/phiMny = 43.143 / 342.599 = 0.1259`
- `Mz/phiMnz = 0.045 / 342.599 = 0.0001`
- `Suma flexion biaxial = 0.1261`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`
- `D/C = 0.1294`
- Estado: **Cumple**

Nota: esta seccion resume la revision global historica del framework. Para columnas OR simples, el dictamen detallado de la seccion 9.4 debe usarse como criterio gobernante de memoria.

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | My (N-m) | Mz (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 406 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 23,744.74 | 43,143.06 | 44.6059 | 0.129362 | Cumple |
| 410 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 20,275.38 | 36,865.88 | 44.6059 | 0.110557 | Cumple |
| 206 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 20,256.56 | 33,823.95 | 31.8376 | 0.101638 | Cumple |
| 306 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 20,312.02 | 33,094.53 | 36.9649 | 0.0995319 | Cumple |
| 106 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 15,042.25 | 31,359.56 | 9.8140 | 0.0936554 | Cumple |
| 407 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 16,385.05 | 30,059.57 | 44.6059 | 0.0901493 | Cumple |
| 210 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 17,668.36 | 29,646.13 | 31.8376 | 0.0890837 | Cumple |
| 310 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 17,933.63 | 29,229.06 | 36.9649 | 0.0879183 | Cumple |
| 411 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 15,556.69 | 28,513.26 | 44.6059 | 0.0855206 | Cumple |
| 110 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 13,570.60 | 28,218.72 | 9.8140 | 0.084283 | Cumple |
| 303 | Columna_1 | ORC 178 x 9.5 | SISMO_DOWN_1 | 46.8008 | 4,590.29 | 7,592.39 | 7.7938 | 0.0750757 | Cumple |
| 307 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 14,824.80 | 24,126.21 | 36.9649 | 0.0725913 | Cumple |
| 315 | Columna_1 | ORC 178 x 9.5 | SISMO_DOWN_1 | 46.8008 | 4,349.12 | 7,206.20 | 7.7938 | 0.071258 | Cumple |
| 311 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 14,326.89 | 23,236.09 | 36.9649 | 0.0699239 | Cumple |
| 314 | Columna_1 | ORC 178 x 9.5 | SISMO_DOWN_1 | 46.8008 | 4,166.17 | 6,959.22 | 7.7938 | 0.0688065 | Cumple |
| 107 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 11,199.83 | 22,911.42 | 9.8140 | 0.068462 | Cumple |
| 403 | Columna_1 | ORC 178 x 9.5 | SISMO_DOWN_1 | 46.8008 | 4,072.14 | 6,903.52 | 9.4048 | 0.0682507 | Cumple |
| 207 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 13,469.55 | 22,582.68 | 31.8376 | 0.0678824 | Cumple |
| 302 | Columna_1 | ORC 178 x 9.5 | SISMO_DOWN_1 | 46.8008 | 4,105.19 | 6,865.63 | 7.7938 | 0.0678804 | Cumple |
| 111 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | 33.1119 | 10,716.34 | 21,895.85 | 9.8140 | 0.0654304 | Cumple |

### 9.4 Diseno Detallado de Columnas Criticas OR

Esta revision aplica al alcance actual de columnas OR simples simetricas. Las columnas IR y OR rellenas quedan preparadas para implementarse como verificadores adicionales. Para la interaccion axial-flexion biaxial se usan demandas concurrentes por combinacion y extremo; no se mezclan maximos independientes de la envolvente.

#### 9.4.1 Identificacion de Columnas Criticas

| Parametro | Valor |
| --- | --- |
| Columnas OR revisadas | 64 |
| Cumplen | 64 |
| No cumplen | 0 |
| Elemento critico por interaccion | 102 |
| Interaccion P-M maxima | 0.391098 |
| Elemento axial max | 106 |
| Pu max (kN) | 1,041.47 |
| Elemento momento max | 406 |
| M max (kN-m) | 43.2721 |
| Criticas unicas | 3 |

| Rol | Elem | Grupo | Perfil | Combo | Ext | Pu (kN) | M (kN-m) | Interaccion | Dictamen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| interaccion_pm | 102 | Columna_1 | ORC 178 x 9.5 | SISMO_DOWN_1 | I | 503.4713 | 4.1603 | 0.391098 | Cumple |
| axial_maximo | 106 | Columna_2 | ORC 254 x 15.9 | GRAV_DISEÑO | I | 1,041.47 | 10.5476 | 0.317576 | Cumple |
| momento_maximo | 406 | Columna_2 | ORC 254 x 15.9 | SISMO_DOWN_1 | J | 200.3782 | 43.2721 | 0.163554 | Cumple |

#### Columna 102 - interaccion_pm

| Parametro | Valor |
| --- | --- |
| 9.4.1 Rol critico | interaccion_pm |
| Elemento | 102 |
| Grupo | Columna_1 |
| Perfil | ORC 178 x 9.5 |
| Combinacion | SISMO_DOWN_1 |
| Extremo | I |
| 9.4.2 Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| E (kg/cm2) | 2,039,696.53 |
| A (cm2) | 57.8700 |
| Ix = Iy (cm4) | 2,705.50 |
| rx = ry (cm) | 6.8300 |
| J (cm4) | 4,370.40 |
| Dimension exterior (mm) | 178.0000 |
| Ancho plano b (mm) | 152.1900 |
| Espesor t (mm) | 8.9000 |
| 9.4.3 b/t | 17.1000 |
| lambda compacto | 28.1183 |
| lambda no compacto | 35.1479 |
| Clasificacion local | compacta |
| Tipo elemento local | pared atiesada en compresion |
| 9.4.4 Kx | 1.0000 |
| Ky | 1.0000 |
| L (m) | 3.2000 |
| KL/r x | 46.8521 |
| KL/r y | 46.8521 |
| Fcr x (MPa) | 273.7792 |
| Fcr y (MPa) | 273.7792 |
| Ae (cm2) | 57.8700 |
| Q local | 1.0000 |
| Pn control (kN) | 1,584.36 |
| PR (kN) | 1,425.92 |
| Estado limite axial | pandeo_x |
| 9.4.5 S (cm3) | 304.8000 |
| Z (cm3) | 362.2000 |
| MRx (kN-m) | 103.4509 |
| MRy (kN-m) | 103.4509 |
| Estado limite flexion | fluencia_plastica |
| 9.4.6 Vy max (kN) | 2.5024 |
| Vz max (kN) | 0.273301 |
| VRy = VRz (kN) | 542.9715 |
| Cv cortante | 1.0000 |
| 9.4.7 Pu concurrente (kN) | 503.4713 |
| Mux concurrente (kN-m) | 0.272782 |
| Muy concurrente (kN-m) | 4.1514 |
| Pu/PR | 0.353084 |
| Mux/MRx | 0.00263683 |
| Muy/MRy | 0.0401287 |
| Interaccion P-M | 0.391098 |
| Ecuacion | Pu/PR >= 0.20: Pu/PR + 8/9(Mux/MRx + Muy/MRy) |
| 9.4.8 Limite esbeltez | 200.0000 |
| Cumple esbeltez | Si |
| P-Delta global | Incluido en columnas mediante transformacion PDelta del modelo OpenSees. |
| P-delta local | No se amplifica adicionalmente; revisar si se requiere metodo B1/B2 especifico. |
| 9.4.9 Requisitos sismicos | Continuidad de columna y compatibilidad de nudos se verifican en constructibilidad; placas, diafragmas o rigidizadores requieren revision de conexion. |
| 9.4.10 Dictamen | Cumple |

Estados limite revisados:

| Estado | Nombre | Inciso | Aplica | Res diseno | Util | Resultado |
| --- | --- | --- | --- | --- | --- | --- |
| FG | Fluencia general | NTC-Acero 6.3.1 | Si | 1,652,872.23 | 0.304604 | Cumple |
| PX | Pandeo por flexion eje x | NTC-Acero 6.3.1 | Si | 1,425,924.36 | 0.353084 | Cumple |
| PY | Pandeo por flexion eje y | NTC-Acero 6.3.1 | Si | 1,425,924.36 | 0.353084 | Cumple |
| PT | Pandeo torsional/flexotorsional | NTC-Acero 6.3.2 | No | 1,425,924.36 | 0.353084 | Cumple |
| MX | Flexion eje x | NTC-Acero 7 | Si | 103,450.89 | 0.00263683 | Cumple |
| MY | Flexion eje y | NTC-Acero 7 | Si | 103,450.89 | 0.0401287 | Cumple |
| VY | Cortante local y | NTC-Acero 7.10 | Si | 542,971.53 | 0.0046088 | Cumple |
| VZ | Cortante local z | NTC-Acero 7.10 | Si | 542,971.53 | 0.000503343 | Cumple |

#### Columna 106 - axial_maximo

| Parametro | Valor |
| --- | --- |
| 9.4.1 Rol critico | axial_maximo |
| Elemento | 106 |
| Grupo | Columna_2 |
| Perfil | ORC 254 x 15.9 |
| Combinacion | GRAV_DISEÑO |
| Extremo | I |
| 9.4.2 Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| E (kg/cm2) | 2,039,696.53 |
| A (cm2) | 135.4800 |
| Ix = Iy (cm4) | 12,653.40 |
| rx = ry (cm) | 9.6500 |
| J (cm4) | 20,728.30 |
| Dimension exterior (mm) | 254.0000 |
| Ancho plano b (mm) | 210.1600 |
| Espesor t (mm) | 14.8000 |
| 9.4.3 b/t | 14.2000 |
| lambda compacto | 28.1183 |
| lambda no compacto | 35.1479 |
| Clasificacion local | compacta |
| Tipo elemento local | pared atiesada en compresion |
| 9.4.4 Kx | 1.0000 |
| Ky | 1.0000 |
| L (m) | 3.2000 |
| KL/r x | 33.1606 |
| KL/r y | 33.1606 |
| Fcr x (MPa) | 294.7213 |
| Fcr y (MPa) | 294.7213 |
| Ae (cm2) | 135.4800 |
| Q local | 1.0000 |
| Pn control (kN) | 3,992.88 |
| PR (kN) | 3,593.60 |
| Estado limite axial | pandeo_x |
| 9.4.5 S (cm3) | 996.3000 |
| Z (cm3) | 1,199.50 |
| MRx (kN-m) | 342.5990 |
| MRy (kN-m) | 342.5990 |
| Estado limite flexion | fluencia_plastica |
| 9.4.6 Vy max (kN) | 15.0366 |
| Vz max (kN) | 0.188374 |
| VRy = VRz (kN) | 1,288.43 |
| Cv cortante | 1.0000 |
| 9.4.7 Pu concurrente (kN) | 1,041.47 |
| Mux concurrente (kN-m) | 0.153831 |
| Muy concurrente (kN-m) | 10.5465 |
| Pu/PR | 0.289814 |
| Mux/MRx | 0.000449011 |
| Muy/MRy | 0.0307838 |
| Interaccion P-M | 0.317576 |
| Ecuacion | Pu/PR >= 0.20: Pu/PR + 8/9(Mux/MRx + Muy/MRy) |
| 9.4.8 Limite esbeltez | 200.0000 |
| Cumple esbeltez | Si |
| P-Delta global | Incluido en columnas mediante transformacion PDelta del modelo OpenSees. |
| P-delta local | No se amplifica adicionalmente; revisar si se requiere metodo B1/B2 especifico. |
| 9.4.9 Requisitos sismicos | Continuidad de columna y compatibilidad de nudos se verifican en constructibilidad; placas, diafragmas o rigidizadores requieren revision de conexion. |
| 9.4.10 Dictamen | Cumple |

Estados limite revisados:

| Estado | Nombre | Inciso | Aplica | Res diseno | Util | Resultado |
| --- | --- | --- | --- | --- | --- | --- |
| FG | Fluencia general | NTC-Acero 6.3.1 | Si | 3,869,554.70 | 0.269145 | Cumple |
| PX | Pandeo por flexion eje x | NTC-Acero 6.3.1 | Si | 3,593,595.73 | 0.289814 | Cumple |
| PY | Pandeo por flexion eje y | NTC-Acero 6.3.1 | Si | 3,593,595.73 | 0.289814 | Cumple |
| PT | Pandeo torsional/flexotorsional | NTC-Acero 6.3.2 | No | 3,593,595.73 | 0.289814 | Cumple |
| MX | Flexion eje x | NTC-Acero 7 | Si | 342,598.97 | 0.000449011 | Cumple |
| MY | Flexion eje y | NTC-Acero 7 | Si | 342,598.97 | 0.0307838 | Cumple |
| VY | Cortante local y | NTC-Acero 7.10 | Si | 1,288,434.90 | 0.0116704 | Cumple |
| VZ | Cortante local z | NTC-Acero 7.10 | Si | 1,288,434.90 | 0.000146204 | Cumple |

#### Columna 406 - momento_maximo

| Parametro | Valor |
| --- | --- |
| 9.4.1 Rol critico | momento_maximo |
| Elemento | 406 |
| Grupo | Columna_2 |
| Perfil | ORC 254 x 15.9 |
| Combinacion | SISMO_DOWN_1 |
| Extremo | J |
| 9.4.2 Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| E (kg/cm2) | 2,039,696.53 |
| A (cm2) | 135.4800 |
| Ix = Iy (cm4) | 12,653.40 |
| rx = ry (cm) | 9.6500 |
| J (cm4) | 20,728.30 |
| Dimension exterior (mm) | 254.0000 |
| Ancho plano b (mm) | 210.1600 |
| Espesor t (mm) | 14.8000 |
| 9.4.3 b/t | 14.2000 |
| lambda compacto | 28.1183 |
| lambda no compacto | 35.1479 |
| Clasificacion local | compacta |
| Tipo elemento local | pared atiesada en compresion |
| 9.4.4 Kx | 1.0000 |
| Ky | 1.0000 |
| L (m) | 3.2000 |
| KL/r x | 33.1606 |
| KL/r y | 33.1606 |
| Fcr x (MPa) | 294.7213 |
| Fcr y (MPa) | 294.7213 |
| Ae (cm2) | 135.4800 |
| Q local | 1.0000 |
| Pn control (kN) | 3,992.88 |
| PR (kN) | 3,593.60 |
| Estado limite axial | pandeo_x |
| 9.4.5 S (cm3) | 996.3000 |
| Z (cm3) | 1,199.50 |
| MRx (kN-m) | 342.5990 |
| MRy (kN-m) | 342.5990 |
| Estado limite flexion | fluencia_plastica |
| 9.4.6 Vy max (kN) | 23.7408 |
| Vz max (kN) | 1.8939 |
| VRy = VRz (kN) | 1,288.43 |
| Cv cortante | 1.0000 |
| 9.4.7 Pu concurrente (kN) | 200.3782 |
| Mux concurrente (kN-m) | 3.3388 |
| Muy concurrente (kN-m) | 43.1431 |
| Pu/PR | 0.0557598 |
| Mux/MRx | 0.00974546 |
| Muy/MRy | 0.125929 |
| Interaccion P-M | 0.163554 |
| Ecuacion | Pu/PR < 0.20: Pu/(2PR) + Mux/MRx + Muy/MRy |
| 9.4.8 Limite esbeltez | 200.0000 |
| Cumple esbeltez | Si |
| P-Delta global | Incluido en columnas mediante transformacion PDelta del modelo OpenSees. |
| P-delta local | No se amplifica adicionalmente; revisar si se requiere metodo B1/B2 especifico. |
| 9.4.9 Requisitos sismicos | Continuidad de columna y compatibilidad de nudos se verifican en constructibilidad; placas, diafragmas o rigidizadores requieren revision de conexion. |
| 9.4.10 Dictamen | Cumple |

Estados limite revisados:

| Estado | Nombre | Inciso | Aplica | Res diseno | Util | Resultado |
| --- | --- | --- | --- | --- | --- | --- |
| FG | Fluencia general | NTC-Acero 6.3.1 | Si | 3,869,554.70 | 0.0517833 | Cumple |
| PX | Pandeo por flexion eje x | NTC-Acero 6.3.1 | Si | 3,593,595.73 | 0.0557598 | Cumple |
| PY | Pandeo por flexion eje y | NTC-Acero 6.3.1 | Si | 3,593,595.73 | 0.0557598 | Cumple |
| PT | Pandeo torsional/flexotorsional | NTC-Acero 6.3.2 | No | 3,593,595.73 | 0.0557598 | Cumple |
| MX | Flexion eje x | NTC-Acero 7 | Si | 342,598.97 | 0.00974546 | Cumple |
| MY | Flexion eje y | NTC-Acero 7 | Si | 342,598.97 | 0.125929 | Cumple |
| VY | Cortante local y | NTC-Acero 7.10 | Si | 1,288,434.90 | 0.0184261 | Cumple |
| VZ | Cortante local z | NTC-Acero 7.10 | Si | 1,288,434.90 | 0.00146993 | Cumple |

#### Resumen de Columnas OR

| Elem | Grupo | Perfil | Pu (kN) | M (kN-m) | KL/r | P-M | Vy | Vz | Control | Dictamen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 102 | Columna_1 | ORC 178 x 9.5 | 503.4713 | 4.1603 | 46.8521 | 0.391098 | 0.0046088 | 0.000503343 | 0.391098 | Cumple |
| 114 | Columna_1 | ORC 178 x 9.5 | 496.2806 | 4.0984 | 46.8521 | 0.385881 | 0.00453001 | 0.000514627 | 0.385881 | Cumple |
| 115 | Columna_1 | ORC 178 x 9.5 | 452.5275 | 4.2168 | 46.8521 | 0.355485 | 0.004713 | 0.000352581 | 0.355485 | Cumple |
| 103 | Columna_1 | ORC 178 x 9.5 | 441.9732 | 4.3250 | 46.8521 | 0.349992 | 0.00481977 | 0.000576344 | 0.349992 | Cumple |
| 106 | Columna_2 | ORC 254 x 15.9 | 1,036.85 | 21.3746 | 33.1606 | 0.344037 | 0.0116704 | 0.000146204 | 0.344037 | Cumple |
| 105 | Columna_1 | ORC 178 x 9.5 | 471.8955 | 1.0582 | 46.8521 | 0.343748 | 0.000599506 | 0.00426776 | 0.343748 | Cumple |
| 202 | Columna_1 | ORC 178 x 9.5 | 400.3533 | 5.5664 | 46.8521 | 0.33663 | 0.00611732 | 0.00108073 | 0.33663 | Cumple |
| 214 | Columna_1 | ORC 178 x 9.5 | 394.4836 | 5.6405 | 46.8521 | 0.334551 | 0.00627589 | 0.00149578 | 0.334551 | Cumple |
| 109 | Columna_1 | ORC 178 x 9.5 | 459.1067 | 0.945173 | 46.8521 | 0.333444 | 0.000617199 | 0.00412633 | 0.333444 | Cumple |
| 110 | Columna_2 | ORC 254 x 15.9 | 987.8371 | 17.4803 | 33.1606 | 0.321201 | 0.010529 | 0.000215038 | 0.321201 | Cumple |
| 203 | Columna_1 | ORC 178 x 9.5 | 349.0753 | 6.1313 | 46.8521 | 0.307878 | 0.00686533 | 0.00160922 | 0.307878 | Cumple |
| 215 | Columna_1 | ORC 178 x 9.5 | 358.5629 | 5.7106 | 46.8521 | 0.307786 | 0.00646473 | 0.00108096 | 0.307786 | Cumple |
| 206 | Columna_2 | ORC 254 x 15.9 | 780.5459 | 30.0467 | 33.1606 | 0.298323 | 0.0157146 | 0.00082027 | 0.298323 | Cumple |
| 113 | Columna_1 | ORC 178 x 9.5 | 365.4316 | 3.4397 | 46.8521 | 0.286437 | 0.000120969 | 0.00338524 | 0.286437 | Cumple |
| 107 | Columna_2 | ORC 254 x 15.9 | 774.4632 | 19.0704 | 33.1606 | 0.28394 | 0.00869578 | 0.00578233 | 0.28394 | Cumple |
| 101 | Columna_1 | ORC 178 x 9.5 | 350.0919 | 3.5034 | 46.8521 | 0.276559 | 0.000190452 | 0.0034272 | 0.276559 | Cumple |
| 108 | Columna_1 | ORC 178 x 9.5 | 340.4918 | 3.8853 | 46.8521 | 0.274606 | 0.00332083 | 0.00404875 | 0.274606 | Cumple |
| 210 | Columna_2 | ORC 254 x 15.9 | 743.7516 | 24.7401 | 33.1606 | 0.273058 | 0.01371 | 0.000641698 | 0.273058 | Cumple |
| 116 | Columna_1 | ORC 178 x 9.5 | 315.4887 | 3.4705 | 46.8521 | 0.251901 | 0.000225315 | 0.0034379 | 0.251901 | Cumple |
| 104 | Columna_1 | ORC 178 x 9.5 | 309.1624 | 3.5509 | 46.8521 | 0.249247 | 0.000412973 | 0.00349979 | 0.249247 | Cumple |
| 205 | Columna_1 | ORC 178 x 9.5 | 289.6501 | 4.6708 | 46.8521 | 0.248897 | 0.00072729 | 0.00510307 | 0.248897 | Cumple |
| 213 | Columna_1 | ORC 178 x 9.5 | 306.1921 | 3.0141 | 46.8521 | 0.24672 | 0.00120306 | 0.0030612 | 0.24672 | Cumple |
| 209 | Columna_1 | ORC 178 x 9.5 | 316.9784 | 1.2646 | 46.8521 | 0.237587 | 0.00103372 | 0.00480819 | 0.237587 | Cumple |
| 201 | Columna_1 | ORC 178 x 9.5 | 290.9473 | 2.9159 | 46.8521 | 0.234183 | 0.00106798 | 0.0030175 | 0.234183 | Cumple |
| 207 | Columna_2 | ORC 254 x 15.9 | 585.2240 | 26.1184 | 33.1606 | 0.187709 | 0.0104595 | 0.0101289 | 0.187709 | Cumple |
| 407 | Columna_2 | ORC 254 x 15.9 | 151.2700 | 40.2893 | 33.1606 | 0.187089 | 0.0127193 | 0.0132343 | 0.187089 | Cumple |
| 208 | Columna_1 | ORC 178 x 9.5 | 220.5876 | 6.5561 | 46.8521 | 0.1662 | 0.00683256 | 0.00475123 | 0.1662 | Cumple |
| 406 | Columna_2 | ORC 254 x 15.9 | 200.3782 | 43.2721 | 33.1606 | 0.163554 | 0.0184261 | 0.00146993 | 0.163554 | Cumple |
| 306 | Columna_2 | ORC 254 x 15.9 | 409.1758 | 33.0996 | 33.1606 | 0.158496 | 0.0157591 | 0.000805048 | 0.158496 | Cumple |
| 307 | Columna_2 | ORC 254 x 15.9 | 308.2427 | 28.4575 | 33.1606 | 0.157418 | 0.0115103 | 0.00826918 | 0.157418 | Cumple |

#### Alertas

_Sin alertas generales._

### 9.5 Diseno Axial LRFD de Contravientos OR

Los contravientos OR se revisan con la demanda axial de la envolvente LRFD. Debido a que la respuesta espectral CQC se reporta como envolvente positiva, la misma demanda axial se verifica conservadoramente contra tension y contra compresion.

#### Resumen General

| Parametro | Valor |
| --- | --- |
| Contravientos OR revisados | 64 |
| Cumplen | 64 |
| No cumplen | 0 |
| Elemento critico | 367 |
| Grupo critico | Contraviento_2 |
| Perfil critico | ORC 76 x 6.4 |
| Combo critico | SISMO_DOWN_1 |
| Estado control | compresion |
| P max (kN) | 77.4363 |
| PR compresion (kN) | 97.6202 |
| TR tension (kN) | 448.7061 |
| Utilizacion max | 0.793241 |

#### Calculo del Contraviento Critico

| Paso / Propiedad | Valor |
| --- | --- |
| Elemento | 367 |
| Grupo | Contraviento_2 |
| Perfil | ORC 76 x 6.4 |
| Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| A (cm2) | 15.7100 |
| b ext (mm) | 76.0000 |
| t (mm) | 5.9000 |
| lambda pared | 9.9000 |
| Clasificacion local | compacta |
| K | 1.0000 |
| L (m) | 4.4973 |
| KL/r | 158.3552 |
| Limite esbeltez | 200.0000 |
| P demanda (kN) | 77.4363 |
| PR compresion (kN) | 97.6202 |
| TR tension (kN) | 448.7061 |
| Util compresion | 0.793241 |
| Util tension | 0.172577 |
| Control | compresion |
| Estado | Cumple |

#### Resumen de Contravientos Revisados

| Elem | Grupo | Perfil | P (kN) | PR comp (kN) | TR tens (kN) | Util comp | Util tens | Control | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 367 | Contraviento_2 | ORC 76 x 6.4 | 77.4363 | 97.6202 | 448.7061 | 0.793241 | 0.172577 | compresion | Cumple |
| 355 | Contraviento_2 | ORC 76 x 6.4 | 77.0419 | 97.6202 | 448.7061 | 0.789201 | 0.171698 | compresion | Cumple |
| 356 | Contraviento_2 | ORC 76 x 6.4 | 64.6597 | 97.6202 | 448.7061 | 0.66236 | 0.144103 | compresion | Cumple |
| 368 | Contraviento_2 | ORC 76 x 6.4 | 62.2905 | 97.6202 | 448.7061 | 0.638091 | 0.138823 | compresion | Cumple |
| 358 | Contraviento_2 | ORC 76 x 6.4 | 70.8928 | 111.7666 | 448.7061 | 0.634293 | 0.157994 | compresion | Cumple |
| 370 | Contraviento_2 | ORC 76 x 6.4 | 67.9307 | 111.9391 | 448.7061 | 0.606854 | 0.151392 | compresion | Cumple |
| 468 | Contraviento_2 | ORC 76 x 6.4 | 58.4581 | 97.6202 | 448.7061 | 0.598832 | 0.130281 | compresion | Cumple |
| 456 | Contraviento_2 | ORC 76 x 6.4 | 56.8986 | 97.6202 | 448.7061 | 0.582857 | 0.126806 | compresion | Cumple |
| 457 | Contraviento_2 | ORC 76 x 6.4 | 54.7214 | 111.7666 | 448.7061 | 0.489604 | 0.121954 | compresion | Cumple |
| 369 | Contraviento_2 | ORC 76 x 6.4 | 52.8326 | 111.5943 | 448.7061 | 0.473434 | 0.117744 | compresion | Cumple |
| 357 | Contraviento_2 | ORC 76 x 6.4 | 52.3600 | 111.7666 | 448.7061 | 0.468477 | 0.116691 | compresion | Cumple |
| 469 | Contraviento_2 | ORC 76 x 6.4 | 51.2089 | 111.5943 | 448.7061 | 0.458885 | 0.114126 | compresion | Cumple |
| 467 | Contraviento_2 | ORC 76 x 6.4 | 36.6047 | 97.6202 | 448.7061 | 0.374971 | 0.0815784 | compresion | Cumple |
| 455 | Contraviento_2 | ORC 76 x 6.4 | 35.6020 | 97.6202 | 448.7061 | 0.364699 | 0.0793437 | compresion | Cumple |
| 458 | Contraviento_2 | ORC 76 x 6.4 | 35.4544 | 111.7666 | 448.7061 | 0.317218 | 0.0790147 | compresion | Cumple |
| 156 | Contraviento_1 | ORC 127 x 6.4 | 133.7165 | 450.0644 | 791.4479 | 0.297105 | 0.168952 | compresion | Cumple |
| 268 | Contraviento_1 | ORC 127 x 6.4 | 133.0779 | 450.0644 | 791.4479 | 0.295686 | 0.168145 | compresion | Cumple |
| 168 | Contraviento_1 | ORC 127 x 6.4 | 132.1988 | 450.0644 | 791.4479 | 0.293733 | 0.167034 | compresion | Cumple |
| 256 | Contraviento_1 | ORC 127 x 6.4 | 131.6209 | 450.0644 | 791.4479 | 0.292449 | 0.166304 | compresion | Cumple |
| 167 | Contraviento_1 | ORC 127 x 6.4 | 125.8073 | 450.0644 | 791.4479 | 0.279532 | 0.158958 | compresion | Cumple |
| 155 | Contraviento_1 | ORC 127 x 6.4 | 125.8036 | 450.0644 | 791.4479 | 0.279523 | 0.158954 | compresion | Cumple |
| 470 | Contraviento_2 | ORC 76 x 6.4 | 30.9708 | 111.9391 | 448.7061 | 0.276676 | 0.0690225 | compresion | Cumple |
| 257 | Contraviento_1 | ORC 127 x 6.4 | 119.3890 | 483.3962 | 791.4479 | 0.24698 | 0.150849 | compresion | Cumple |
| 169 | Contraviento_1 | ORC 127 x 6.4 | 118.8413 | 483.0284 | 791.4479 | 0.246034 | 0.150157 | compresion | Cumple |
| 157 | Contraviento_1 | ORC 127 x 6.4 | 118.2509 | 483.3962 | 791.4479 | 0.244625 | 0.149411 | compresion | Cumple |
| 255 | Contraviento_1 | ORC 127 x 6.4 | 108.6992 | 450.0644 | 791.4479 | 0.241519 | 0.137342 | compresion | Cumple |
| 269 | Contraviento_1 | ORC 127 x 6.4 | 116.5837 | 483.0284 | 791.4479 | 0.24136 | 0.147304 | compresion | Cumple |
| 158 | Contraviento_1 | ORC 127 x 6.4 | 114.4249 | 483.3962 | 791.4479 | 0.23671 | 0.144577 | compresion | Cumple |
| 170 | Contraviento_1 | ORC 127 x 6.4 | 114.3394 | 483.7637 | 791.4479 | 0.236354 | 0.144469 | compresion | Cumple |
| 267 | Contraviento_1 | ORC 127 x 6.4 | 106.1704 | 450.0644 | 791.4479 | 0.2359 | 0.134147 | compresion | Cumple |

#### Alertas

_Sin alertas generales._

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 330 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 63,887.05 | 240,823.34 | 0.265286 | Cumple |
| 230 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 63,556.79 | 240,823.34 | 0.263915 | Cumple |
| 130 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 63,227.26 | 240,823.34 | 0.262546 | Cumple |
| 430 | Viga_2 | IR 356 x 44.6 | GRAV_DISEÑO | 56,403.12 | 240,823.34 | 0.23421 | Cumple |
| 431 | Viga_2 | IR 356 x 44.6 | GRAV_DISEÑO | 53,680.86 | 240,823.34 | 0.222906 | Cumple |
| 237 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 53,661.46 | 240,823.34 | 0.222825 | Cumple |
| 337 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 53,350.01 | 240,823.34 | 0.221532 | Cumple |
| 137 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 52,386.59 | 240,823.34 | 0.217531 | Cumple |
| 231 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 50,829.64 | 240,823.34 | 0.211066 | Cumple |
| 331 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 50,308.61 | 240,823.34 | 0.208903 | Cumple |
| 131 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 49,118.20 | 240,823.34 | 0.203959 | Cumple |
| 437 | Viga_2 | IR 356 x 44.6 | GRAV_DISEÑO | 46,131.29 | 240,823.34 | 0.191557 | Cumple |

### 10.2 Contravientos Criticos

| Elem | Grupo | Perfil | Combo | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 367 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 77,436.34 | 0 | 0.793307 | Cumple |
| 355 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 77,041.91 | 0 | 0.789267 | Cumple |
| 356 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 64,659.69 | 0 | 0.662415 | Cumple |
| 368 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 62,290.53 | 0 | 0.638144 | Cumple |
| 358 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 70,892.79 | 0 | 0.634346 | Cumple |
| 370 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 67,930.72 | 0 | 0.606905 | Cumple |
| 468 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 58,458.08 | 0 | 0.598882 | Cumple |
| 456 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 56,898.57 | 0 | 0.582905 | Cumple |
| 457 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 54,721.39 | 0 | 0.489645 | Cumple |
| 369 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 52,832.58 | 0 | 0.473474 | Cumple |
| 357 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 52,360.03 | 0 | 0.468516 | Cumple |
| 469 | Contraviento_2 | ORC 76 x 6.4 | SISMO_DOWN_1 | 51,208.94 | 0 | 0.458923 | Cumple |

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
