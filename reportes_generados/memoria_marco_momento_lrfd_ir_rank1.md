# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-06-16 23:35:18

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 100 |
| Elementos | 184 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 55,401.88 |
| Masa sismica (kg) | 646,948.29 |

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
| CM total (kgf) | 490,450.72 |
| CV instantanea total (kgf) | 101,095.69 |
| Masa propia elementos (kg) | 55,401.88 |
| Masa sismica total (kg) | 646,948.29 |
| Peso sismico total (N) | 6,346,562.72 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 2,895.36 | 2,895.36 |
| 3.2000 | 127,052.69 | 27,323.16 | 14,622.31 | 168,998.16 |
| 6.4000 | 127,052.69 | 27,323.16 | 14,622.31 | 168,998.16 |
| 9.6000 | 127,052.69 | 27,323.16 | 14,622.31 | 168,998.16 |
| 12.8000 | 109,292.64 | 19,126.21 | 8,639.60 | 137,058.45 |

## 5. Analisis Modal

El numero de modos se selecciona buscando alcanzar la participacion modal minima configurada.

| Parametro | Valor |
| --- | --- |
| Objetivo participacion | 0.9 |
| Modos usados | 6 |
| Maximo modos |  |

### 5.1 Periodos

| Modo | Periodo (s) |
| --- | --- |
| 1 | 0.854969 |
| 2 | 0.842609 |
| 3 | 0.764503 |
| 4 | 0.261918 |
| 5 | 0.250229 |
| 6 | 0.23344 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.566253 |
| 2 |  |  | 0.767389 |
| 3 |  |  | 0.767874 |
| 4 |  |  | 0.770949 |
| 5 |  |  | 0.91484 |
| 6 |  |  | 0.915096 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.0246847 |
| 2 |  |  | 0.116659 |
| 3 |  |  | 0.775339 |
| 4 |  |  | 0.799778 |
| 5 |  |  | 0.801171 |
| 6 |  |  | 0.916431 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.353765 | 1.0000 | Cumple |
| Deriva servicio | 0.00184095 | 0.005 | Cumple |
| Deriva colapso | 0.008182 | 0.01 | Cumple |
| Desp. azotea | 0.0206825 | 0.1 | Cumple |
| KL/r | 65.6650 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000987854 | 0.00439046 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.00179796 | 0.00799093 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.00183651 | 0.00816228 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.00184095 | 0.008182 |

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
| Ratio max | 0.20671 |
| Flecha maxima (mm) | 4.5649 |
| Flecha admisible (mm) | 22.0833 |
| Estado global | Cumple |

#### Claros Criticos

| Claro | Nivel (m) | Grupo | Perfil | Elementos | L (m) | delta max (mm) | delta adm (mm) | Ratio | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DV-0024 | 12.8000 | Viga_2 | IR 254 x 38.7 | 426, 427 | 5.3000 | 4.5649 | 22.0833 | 0.20671 | Cumple |
| DV-0018 | 12.8000 | Viga_2 | IR 254 x 38.7 | 432, 433 | 5.3000 | 3.8891 | 22.0833 | 0.176112 | Cumple |
| DV-0010 | 12.8000 | Viga_2 | IR 254 x 38.7 | 441, 442 | 6.1000 | 4.1075 | 25.4167 | 0.161607 | Cumple |
| DV-0026 | 12.8000 | Viga_2 | IR 254 x 38.7 | 443, 446 | 9.2500 | 3.2875 | 38.5417 | 0.0852966 | Cumple |
| DV-0076 | 6.4000 | Viga_1 | IR 457 x 59.5 | 226, 227 | 5.3000 | 1.5571 | 22.0833 | 0.0705096 | Cumple |
| DV-0050 | 3.2000 | Viga_1 | IR 457 x 59.5 | 126, 127 | 5.3000 | 1.5532 | 22.0833 | 0.070334 | Cumple |
| DV-0102 | 9.6000 | Viga_1 | IR 457 x 59.5 | 326, 327 | 5.3000 | 1.5096 | 22.0833 | 0.0683608 | Cumple |
| DV-0070 | 6.4000 | Viga_1 | IR 457 x 59.5 | 232, 233 | 5.3000 | 1.3302 | 22.0833 | 0.060236 | Cumple |
| DV-0044 | 3.2000 | Viga_1 | IR 457 x 59.5 | 132, 133 | 5.3000 | 1.3222 | 22.0833 | 0.0598713 | Cumple |
| DV-0096 | 9.6000 | Viga_1 | IR 457 x 59.5 | 332, 333 | 5.3000 | 1.2922 | 22.0833 | 0.0585136 | Cumple |
| DV-0088 | 9.6000 | Viga_1 | IR 457 x 59.5 | 341, 342 | 6.1000 | 1.3072 | 25.4167 | 0.0514299 | Cumple |
| DV-0062 | 6.4000 | Viga_1 | IR 457 x 59.5 | 241, 242 | 6.1000 | 1.2980 | 25.4167 | 0.051068 | Cumple |

#### Alertas

- 88 claros no tienen nodos intermedios; la flecha local puede estar subestimada.

Nota: en claros sin nodos intermedios el indicador nodal puede subestimar la flecha maxima local; esos casos deben refinarse si controlan la revision.

### 8.5 Reacciones de Base para Cimentacion Preliminar

Se reportan las reacciones en los nodos empotrados del nivel base para apoyar el predimensionamiento de cimentacion. Las componentes `R` corresponden a la reaccion del apoyo sobre la estructura; la accion transmitida a la cimentacion tiene signo opuesto.

En combinaciones gravitacionales las reacciones se conservan con signo. En combinaciones espectrales CQC se reporta una envolvente absoluta de gravedad mas sismo, consistente con el criterio usado para fuerzas internas LRFD.

#### Resumen por Combinacion

| Combo | Criterio | Nodos | P total (kN) | Vh (kN) | Mx (kN-m) | My (kN-m) | M vol. (kN-m) | P max nodo (kN) | Uplift total (kN) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GRAV_DISEÑO | gravedad_signada | 16 | 9,803.26 | 5.14613e-11 | -1,285.21 | 7,369.68 | 7,480.91 | 1,216.99 | 0 |
| SISMO_DOWN_1 | envolvente_abs_grav_cqc | 16 | 7,636.73 | 453.6009 | -1,732.16 | 6,218.98 | 6,455.70 | 879.8233 | 0 |
| SISMO_DOWN_2 | envolvente_abs_grav_cqc | 16 | 7,636.73 | 453.6009 | -1,732.16 | 6,218.98 | 6,455.70 | 879.8233 | 0 |
| SISMO_DOWN_3 | envolvente_abs_grav_cqc | 16 | 7,636.73 | 453.6009 | -1,732.16 | 6,218.98 | 6,455.70 | 879.8233 | 0 |
| SISMO_DOWN_4 | envolvente_abs_grav_cqc | 16 | 7,636.73 | 453.6009 | -1,732.16 | 6,218.98 | 6,455.70 | 879.8233 | 0 |
| GRAV_SERVICIO | gravedad_signada | 16 | 7,249.48 | 1.50167e-11 | -938.5264 | 5,454.46 | 5,534.62 | 897.4807 | 0 |
| SISMO_DOWN_5 | envolvente_abs_grav_cqc | 16 | 7,202.84 | 168.1071 | -1,021.86 | 5,669.34 | 5,760.69 | 857.1014 | 0 |
| SISMO_DOWN_6 | envolvente_abs_grav_cqc | 16 | 7,202.84 | 168.1071 | -1,021.86 | 5,669.34 | 5,760.69 | 857.1014 | 0 |
| SISMO_DOWN_7 | envolvente_abs_grav_cqc | 16 | 7,202.84 | 168.1071 | -1,021.86 | 5,669.34 | 5,760.69 | 857.1014 | 0 |
| SISMO_DOWN_8 | envolvente_abs_grav_cqc | 16 | 7,202.84 | 168.1071 | -1,021.86 | 5,669.34 | 5,760.69 | 857.1014 | 0 |
| SISMO_UP_1 | envolvente_abs_grav_cqc | 16 | 5,474.84 | 447.6608 | -1,487.99 | 4,495.80 | 4,735.64 | 614.5583 | 0 |
| SISMO_UP_2 | envolvente_abs_grav_cqc | 16 | 5,474.84 | 447.6608 | -1,487.99 | 4,495.80 | 4,735.64 | 614.5583 | 0 |

#### Envolvente por Nodo de Base

| Nodo | X (m) | Y (m) | P max (kN) | Combo P | Uplift (kN) | Vh max (kN) | Combo V | M nodo (kN-m) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 6 | 6.3200 | 6.2000 | 1,216.99 | GRAV_DISEÑO | 0 | 13.1706 | SISMO_DOWN_1 | 26.9748 |
| 10 | 6.3200 | 12.3000 | 1,156.77 | GRAV_DISEÑO | 0 | 12.1424 | SISMO_DOWN_1 | 26.1477 |
| 7 | 11.6200 | 6.2000 | 941.9940 | GRAV_DISEÑO | 0 | 16.0082 | SISMO_DOWN_1 | 26.2704 |
| 9 | 0 | 12.3000 | 662.3507 | GRAV_DISEÑO | 0 | 42.0042 | SISMO_DOWN_1 | 135.1744 |
| 5 | 0 | 6.2000 | 656.6251 | GRAV_DISEÑO | 0 | 51.2024 | SISMO_DOWN_1 | 169.1134 |
| 14 | 6.3200 | 18.5000 | 641.3535 | GRAV_DISEÑO | 0 | 8.6439 | SISMO_DOWN_1 | 22.8246 |
| 2 | 6.3200 | 0 | 635.0345 | GRAV_DISEÑO | 0 | 11.2178 | SISMO_DOWN_1 | 25.1529 |
| 11 | 11.6200 | 12.3000 | 617.1911 | GRAV_DISEÑO | 0 | 10.3503 | SISMO_DOWN_1 | 26.2939 |
| 15 | 11.6200 | 18.5000 | 593.1715 | GRAV_DISEÑO | 0 | 8.7179 | SISMO_DOWN_1 | 25.4472 |
| 3 | 11.6200 | 0 | 576.6636 | GRAV_DISEÑO | 0 | 12.0111 | SISMO_DOWN_1 | 28.3863 |
| 8 | 17.0700 | 6.2000 | 470.3072 | GRAV_DISEÑO | 0 | 51.1920 | SISMO_DOWN_1 | 168.2685 |
| 13 | 0 | 18.5000 | 362.8784 | GRAV_DISEÑO | 0 | 35.0345 | SISMO_DOWN_1 | 108.7201 |
| 1 | 0 | 0 | 356.9768 | GRAV_DISEÑO | 0 | 59.5404 | SISMO_UP_1 | 204.0682 |
| 16 | 17.0700 | 18.5000 | 316.9748 | GRAV_DISEÑO | 0 | 34.9796 | SISMO_DOWN_1 | 108.3023 |
| 4 | 17.0700 | 0 | 305.5335 | GRAV_DISEÑO | 0 | 62.0102 | SISMO_DOWN_1 | 207.0541 |
| 12 | 17.0700 | 12.3000 | 292.4525 | GRAV_DISEÑO | 0 | 42.6547 | SISMO_DOWN_1 | 136.2717 |

#### Alertas

_Sin alertas._

### 8.6 Fuerzas Internas para Diseno LRFD

Se exportan fuerzas locales por elemento, combinacion y extremo para uso en revisiones LRFD. Las combinaciones de gravedad se reportan con signo local. En combinaciones sismicas por CQC se reporta una envolvente positiva de diseno, ya que el metodo modal espectral no conserva un signo concurrente unico.

| Parametro | Valor |
| --- | --- |
| Elementos | 184 |
| Combinaciones | 18 |
| Filas detalle | 6624 |
| Filas envolvente | 184 |
| Filas envolvente grupos | 3 |
| Incluye CQC | Si |
| Elemento momento critico | 104 |
| M max (kN-m) | 207.0541 |
| Combo momento critico | SISMO_DOWN_1 |
| Elemento axial critico | 106 |
| P max (kN) | 1,216.99 |
| Combo axial critico | GRAV_DISEÑO |
| Grupo momento critico | Columna_1 |
| Grupo M max (kN-m) | 207.0541 |
| Grupo axial critico | Columna_1 |
| Grupo P max (kN) | 1,216.99 |

#### Envolvente por Grupo de Diseno

| Grupo | Tipo | Perfiles | Elems | P max (kN) | Elem P | Combo P | V max (kN) | Elem V | Combo V | M max (kN-m) | Elem M | Combo M |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Columna_1 | COLUMNA | IR 610 x 113.1 | 64 | 1,216.99 | 106 | GRAV_DISEÑO | 62.0782 | 104 | SISMO_DOWN_1 | 207.0541 | 104 | SISMO_DOWN_1 |
| Viga_1 | VIGA | IR 457 x 59.5 | 90 | 4.27654e-13 | 244 | SISMO_DOWN_1 | 91.6197 | 326 | GRAV_DISEÑO | 90.7342 | 221 | SISMO_DOWN_1 |
| Viga_2 | VIGA | IR 254 x 38.7 | 30 | 5.96339e-13 | 444 | SISMO_DOWN_1 | 72.0768 | 426 | GRAV_DISEÑO | 56.3209 | 426 | GRAV_DISEÑO |

#### Envolvente Critica por Momento Resultante

| Elem | Tipo | Grupo | Perfil | M max (kN-m) | Combo M | Extremo M | P max (kN) | Combo P | V max (kN) | Combo V |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 104 | COLUMNA | Columna_1 | IR 610 x 113.1 | 207.0541 | SISMO_DOWN_1 | I | 305.5335 | GRAV_DISEÑO | 62.0782 | SISMO_DOWN_1 |
| 101 | COLUMNA | Columna_1 | IR 610 x 113.1 | 204.0682 | SISMO_DOWN_1 | I | 356.9768 | GRAV_DISEÑO | 59.4891 | SISMO_UP_1 |
| 105 | COLUMNA | Columna_1 | IR 610 x 113.1 | 169.1134 | SISMO_DOWN_1 | I | 656.6251 | GRAV_DISEÑO | 51.1699 | SISMO_DOWN_1 |
| 108 | COLUMNA | Columna_1 | IR 610 x 113.1 | 168.2685 | SISMO_DOWN_1 | I | 470.3072 | GRAV_DISEÑO | 51.2414 | SISMO_DOWN_1 |
| 112 | COLUMNA | Columna_1 | IR 610 x 113.1 | 136.2717 | SISMO_DOWN_1 | I | 292.4525 | GRAV_DISEÑO | 42.6763 | SISMO_DOWN_1 |
| 109 | COLUMNA | Columna_1 | IR 610 x 113.1 | 135.1744 | SISMO_DOWN_1 | I | 662.3507 | GRAV_DISEÑO | 41.9849 | SISMO_DOWN_1 |
| 113 | COLUMNA | Columna_1 | IR 610 x 113.1 | 108.7201 | SISMO_DOWN_1 | I | 362.8784 | GRAV_DISEÑO | 35.0271 | SISMO_DOWN_1 |
| 116 | COLUMNA | Columna_1 | IR 610 x 113.1 | 108.3023 | SISMO_DOWN_1 | I | 316.9748 | GRAV_DISEÑO | 34.9916 | SISMO_DOWN_1 |
| 204 | COLUMNA | Columna_1 | IR 610 x 113.1 | 91.0113 | SISMO_DOWN_1 | I | 225.0312 | GRAV_DISEÑO | 46.3972 | SISMO_DOWN_1 |
| 221 | VIGA | Viga_1 | IR 457 x 59.5 | 90.7342 | SISMO_DOWN_1 | J | 0 |  | 26.8591 | SISMO_DOWN_1 |
| 321 | VIGA | Viga_1 | IR 457 x 59.5 | 84.7581 | SISMO_DOWN_1 | J | 0 |  | 25.1708 | SISMO_DOWN_1 |
| 201 | COLUMNA | Columna_1 | IR 610 x 113.1 | 82.1405 | SISMO_DOWN_1 | I | 262.1923 | GRAV_DISEÑO | 41.6254 | SISMO_DOWN_1 |

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
| Vigas IR revisadas | 120 |
| Cumplen | 120 |
| No cumplen | 0 |
| Vigas controladas por demanda local | 112 |
| Elemento critico | 428 |
| Grupo critico | Viga_2 |
| Perfil critico | IR 254 x 38.7 |
| Combo critico | GRAV_DISEÑO |
| Demanda control | local_gravitacional |
| Mu max (kN-m) | 65.7606 |
| Mu modelo (kN-m) | 4.2548 |
| Mu local (kN-m) | 65.7606 |
| MR control (kN-m) | 67.4371 |
| Utilizacion flexion max | 0.975139 |
| Utilizacion control max | 0.992544 |
| Estado limite control | PLT |

#### Calculo Detallado de la Viga Critica

| Paso / Propiedad | Valor |
| --- | --- |
| 1. Perfil | IR 254 x 38.7 |
| 2. Eje de flexion | x-x IMCA / eje fuerte |
| Elemento | 428 |
| Grupo | Viga_2 |
| Combinacion Mu | GRAV_DISEÑO |
| Extremo Mu | VL-0008 |
| Demanda control | local_gravitacional |
| Mu modelo (kN-m) | 4.2548 |
| Mu local (kN-m) | 65.7606 |
| Claro local | VL-0008 |
| Longitud claro local (m) | 6.1000 |
| w diseno local (kN/m) | 14.1383 |
| Material | A992 |
| Fy (kg/cm2) | 3,515.00 |
| E (kg/cm2) | 2,039,696.53 |
| d (mm) | 262.0000 |
| bf (mm) | 147.0000 |
| tf (mm) | 11.2000 |
| tw (mm) | 6.7000 |
| h (mm) | 224.0000 |
| Ix (cm4) | 5,994.00 |
| Iy (cm4) | 587.0000 |
| Sx (cm3) | 458.0000 |
| Zx (cm3) | 513.0000 |
| ry (cm) | 3.5000 |
| J (cm4) | 17.0000 |
| Cw (cm6) | 92,645.00 |
| lambda_f = bf / 2tf | 6.5625 |
| lambda_f limite compacto | 9.1523 |
| lambda_f limite no compacto | 24.0850 |
| Clasificacion patin | compacta |
| lambda_w = h / tw | 33.4328 |
| lambda_w limite compacto | 59.0082 |
| lambda_w limite no compacto | 134.8758 |
| Clasificacion alma | compacta |
| 6. Inciso NTC seleccionado | 7.3 |
| Lb (m) | 6.1000 |
| Cb | 1.0000 |
| Lu (m) | 1.4836 |
| Lr (m) | 4.5616 |
| Mu (kN-m) | 65.7606 |
| Vu (kN) | 43.1217 |

#### Estados Limite de Flexion

| Estado | Nombre | Inciso | Aplica | Mn (kN-m) | MR (kN-m) | Util | Resultado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| F | Fluencia | NTC-Acero 7.3.1 / 7.4.2 | Si | 176.8934 | 159.2041 | 0.413058 | Cumple |
| PLT | Pandeo lateral por flexotorsion | NTC-Acero 7.3.2 / 7.4.3 | Si | 74.9301 | 67.4371 | 0.975139 | Cumple |
| PLP | Pandeo local del patin comprimido | NTC-Acero 7.4.4 | No | 176.8934 | 159.2041 | 0.413058 | Cumple |
| PLA | Pandeo local del alma por flexion | NTC-Acero 7.5 | No | 176.8934 | 159.2041 | 0.413058 | Cumple |

#### Verificacion Resistente

| Revision | Valor |
| --- | --- |
| 8. Mn control (kN-m) | 74.9301 |
| Estado limite control | PLT |
| 9. MR = FR Mn (kN-m) | 67.4371 |
| Mu modelo (kN-m) | 4.2548 |
| Mu local (kN-m) | 65.7606 |
| 10. Mu / MR | 0.975139 |
| Vn (kN) | 363.1798 |
| 11. VR (kN) | 326.8618 |
| Vu / VR | 0.131926 |
| 12. Mu/MR + (Vu/VR)^2 | 0.992544 |
| Estado final | Cumple |

#### Referencia de Servicio

| Parametro | Valor |
| --- | --- |
| 13. Claro de deflexion asociado | VL-0008 |
| Ratio flecha | 0.621628 |
| Delta max (mm) | 15.7997 |
| Delta adm (mm) | 25.4167 |
| Estado flecha | Cumple |
| Flecha local calculada (mm) | 15.7997 |
| Flecha local admisible (mm) | 25.4167 |
| Ratio flecha local | 0.621628 |

#### Resumen de Diseno de Vigas IR

| Elem | Grupo | Perfil | Inciso | Demanda | Mu (kN-m) | Mu modelo | Mu local | MR (kN-m) | Vu (kN) | VR (kN) | Flexion | Cortante | F+V | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 428 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 65.7606 | 4.2548 | 65.7606 | 67.4371 | 43.1217 | 326.8618 | 0.975139 | 0.131926 | 0.992544 | Cumple |
| 424 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 60.5685 | 26.6244 | 60.5685 | 64.4797 | 38.3345 | 326.8618 | 0.939342 | 0.117281 | 0.953097 | Cumple |
| 430 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 60.5685 | 22.1648 | 60.5685 | 64.4797 | 38.3345 | 326.8618 | 0.939342 | 0.117281 | 0.953097 | Cumple |
| 420 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 57.5605 | 3.1876 | 57.5605 | 66.0594 | 37.1358 | 326.8618 | 0.871345 | 0.113613 | 0.884253 | Cumple |
| 434 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 57.5605 | 5.9642 | 57.5605 | 66.0594 | 37.1358 | 326.8618 | 0.871345 | 0.113613 | 0.884253 | Cumple |
| 425 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 50.9941 | 3.4331 | 50.9941 | 67.4371 | 33.4388 | 326.8618 | 0.756173 | 0.102302 | 0.766639 | Cumple |
| 422 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 48.7383 | 5.0627 | 48.7383 | 66.0594 | 31.4441 | 326.8618 | 0.737796 | 0.0962 | 0.747051 | Cumple |
| 427 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 75.4360 | 56.2746 | 75.4360 | 114.2693 | 56.9329 | 326.8618 | 0.66016 | 0.17418 | 0.690499 | Cumple |
| 128 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 85.5802 | 11.0881 | 85.5802 | 126.2634 | 56.1182 | 686.2534 | 0.677791 | 0.0817747 | 0.684478 | Cumple |
| 228 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 85.5802 | 13.0604 | 85.5802 | 126.2634 | 56.1182 | 686.2534 | 0.677791 | 0.0817747 | 0.684478 | Cumple |
| 328 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 85.5802 | 12.6521 | 85.5802 | 126.2634 | 56.1182 | 686.2534 | 0.677791 | 0.0817747 | 0.684478 | Cumple |
| 130 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 79.7858 | 47.2875 | 79.7858 | 120.0215 | 50.4974 | 686.2534 | 0.664763 | 0.0735841 | 0.670178 | Cumple |
| 230 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 79.7858 | 51.1030 | 79.7858 | 120.0215 | 50.4974 | 686.2534 | 0.664763 | 0.0735841 | 0.670178 | Cumple |
| 330 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 79.7858 | 47.9520 | 79.7858 | 120.0215 | 50.4974 | 686.2534 | 0.664763 | 0.0735841 | 0.670178 | Cumple |
| 124 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 79.7858 | 60.5731 | 79.7858 | 120.0215 | 50.4974 | 686.2534 | 0.664763 | 0.0735841 | 0.670178 | Cumple |
| 224 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 79.7858 | 66.5855 | 79.7858 | 120.0215 | 50.4974 | 686.2534 | 0.664763 | 0.0735841 | 0.670178 | Cumple |
| 324 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 79.7858 | 60.8401 | 79.7858 | 120.0215 | 50.4974 | 686.2534 | 0.664763 | 0.0735841 | 0.670178 | Cumple |
| 217 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 76.7240 | 76.7240 | 51.7107 | 120.0215 | 32.7283 | 686.2534 | 0.639252 | 0.0476913 | 0.641527 | Cumple |
| 436 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 41.8487 | 5.2690 | 41.8487 | 66.0594 | 26.9992 | 326.8618 | 0.633502 | 0.0826012 | 0.640325 | Cumple |
| 433 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 68.6552 | 48.1238 | 68.6552 | 114.2693 | 51.8152 | 326.8618 | 0.600819 | 0.158523 | 0.625948 | Cumple |
| 120 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 75.7945 | 9.7446 | 75.7945 | 123.3500 | 48.8997 | 686.2534 | 0.614467 | 0.071256 | 0.619545 | Cumple |
| 220 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 75.7945 | 11.3766 | 75.7945 | 123.3500 | 48.8997 | 686.2534 | 0.614467 | 0.071256 | 0.619545 | Cumple |
| 320 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 75.7945 | 10.2965 | 75.7945 | 123.3500 | 48.8997 | 686.2534 | 0.614467 | 0.071256 | 0.619545 | Cumple |
| 134 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 75.7945 | 13.8539 | 75.7945 | 123.3500 | 48.8997 | 686.2534 | 0.614467 | 0.071256 | 0.619545 | Cumple |
| 234 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 75.7945 | 17.6300 | 75.7945 | 123.3500 | 48.8997 | 686.2534 | 0.614467 | 0.071256 | 0.619545 | Cumple |
| 334 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 75.7945 | 18.4120 | 75.7945 | 123.3500 | 48.8997 | 686.2534 | 0.614467 | 0.071256 | 0.619545 | Cumple |
| 426 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 75.4360 | 56.3209 | 75.4360 | 158.8866 | 72.0768 | 326.8618 | 0.474779 | 0.220512 | 0.523405 | Cumple |
| 221 | Viga_1 | IR 457 x 59.5 | 7.3 | local_gravitacional | 90.7342 | 90.7342 | 40.1344 | 148.9678 | 29.4565 | 686.2534 | 0.609086 | 0.0429236 | 0.610928 | Cumple |
| 418 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 39.5947 | 3.9105 | 39.5947 | 66.0594 | 25.5450 | 326.8618 | 0.599381 | 0.0781522 | 0.605489 | Cumple |
| 431 | Viga_2 | IR 254 x 38.7 | 7.3 | local_gravitacional | 39.5947 | 4.6350 | 39.5947 | 66.0594 | 25.5450 | 326.8618 | 0.599381 | 0.0781522 | 0.605489 | Cumple |

#### Alertas

Demanda controlada por revision gravitacional local de claro fisico.

- PLT controla al menos una viga IR con Cb=1.0 y Lb entre nodos; confirmar arriostramiento lateral real del patin comprimido antes de dictaminar el diseno final.
- 88 claros sin nodos intermedios: la revision local usa formulas cerradas de viga simplemente apoyada para capturar momento y flecha maxima.

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
| Elemento | 104 |
| Grupo | Columna_1 |
| Perfil | IR 610 x 113.1 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.01446 |
| Ix (m4) | 0.00087409 |
| Iy (m4) | 3.434e-05 |
| r_min (m) | 0.0487322 |
| KL/r | 65.6650 |
| Pu (kN) | 61.9659 |
| Mu (kN-m) | 207.0140 |
| My local (kN-m) | 207.0140 |
| Mz local (kN-m) | 0.00346658 |
| phi Pn (kN) | 3,274.19 |
| phi Mny (kN-m) | 1,017.29 |
| phi Mnz (kN-m) | 145.5492 |
| Extremo critico | i |
| Tipo revision | IR biaxial: My local contra eje fuerte; Mz local contra eje debil |

Sustitucion numerica:

- `Pu/phiPn = 61.966 / 3274.188 = 0.0189`
- `My/phiMny = 207.014 / 1017.292 = 0.2035`
- `Mz/phiMnz = 0.003 / 145.549 = 0.0000`
- `Suma flexion biaxial = 0.2035`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`
- `D/C = 0.2130`
- Estado: **Cumple**

Nota: esta seccion resume la revision global historica del framework. Para columnas OR simples, el dictamen detallado de las secciones 9.4 y 9.5 debe usarse como criterio gobernante de memoria.

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | My (N-m) | Mz (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 104 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 61,965.87 | 207,013.95 | 3.4666 | 0.212982 | Cumple |
| 101 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 59,482.67 | 204,313.34 | 3.4666 | 0.209948 | Cumple |
| 108 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 51,103.09 | 169,585.30 | 3.4666 | 0.17453 | Cumple |
| 105 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 51,142.76 | 169,062.45 | 3.4666 | 0.174022 | Cumple |
| 112 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 42,612.31 | 136,675.92 | 3.4666 | 0.140884 | Cumple |
| 109 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 41,927.43 | 136,484.51 | 3.4666 | 0.140591 | Cumple |
| 116 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 34,901.15 | 108,842.85 | 3.4666 | 0.112346 | Cumple |
| 113 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 34,957.80 | 108,647.00 | 3.4666 | 0.112162 | Cumple |
| 204 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 46,120.92 | 90,762.49 | 6.2713 | 0.0963059 | Cumple |
| 201 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 41,552.11 | 83,232.70 | 6.2713 | 0.0882064 | Cumple |
| 205 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 36,756.53 | 72,420.52 | 6.2713 | 0.0768456 | Cumple |
| 208 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 36,992.78 | 72,335.16 | 6.2713 | 0.0767978 | Cumple |
| 304 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 32,077.53 | 67,065.40 | 6.1150 | 0.070866 | Cumple |
| 301 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 28,127.48 | 61,270.32 | 6.1150 | 0.0645662 | Cumple |
| 212 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 30,872.65 | 59,149.70 | 6.2713 | 0.0629019 | Cumple |
| 209 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 29,347.81 | 56,878.42 | 6.2713 | 0.0604364 | Cumple |
| 308 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 25,745.79 | 52,980.11 | 6.1150 | 0.0560532 | Cumple |
| 305 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 23,434.15 | 50,705.28 | 6.1150 | 0.053464 | Cumple |
| 213 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 24,169.08 | 49,563.36 | 6.2713 | 0.0524548 | Cumple |
| 216 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | 65.6650 | 24,203.58 | 48,889.33 | 6.2713 | 0.0517975 | Cumple |

### 9.4 Diseno Detallado de Columnas IR

La revision usa demandas P-My-Mz concurrentes por combinacion y extremo. My local se compara con la capacidad del eje fuerte y Mz con la del eje debil. Se incluyen pandeo por flexion en ambos ejes, pandeo torsional, esbeltez local, flexion, cortante e interaccion.

| Parametro | Valor |
| --- | --- |
| Columnas IR revisadas | 64 |
| Cumplen | 64 |
| No cumplen | 0 |
| Interaccion maxima | 0.443033 |
| Elemento critico | 106 |

| Elem | Grupo | Perfil | Combo | Ext | Pu (kN) | Mx (kN-m) | My (kN-m) | KL/r | Q | P-M | Control | Dictamen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 106 | Columna_1 | IR 610 x 113.1 | GRAV_DISEÑO | J | 1,212.37 | 2.1548 | 9.7009 | 65.3061 | 0.952677 | 0.443033 | 0.443033 | Cumple |
| 206 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 647.2009 | 13.2855 | 32.1321 | 65.3061 | 0.952677 | 0.413045 | 0.413045 | Cumple |
| 110 | Columna_1 | IR 610 x 113.1 | GRAV_DISEÑO | J | 1,152.15 | 0.874993 | 8.0888 | 65.3061 | 0.952677 | 0.41297 | 0.41297 | Cumple |
| 107 | Columna_1 | IR 610 x 113.1 | GRAV_DISEÑO | J | 937.3785 | 26.2741 | 6.1082 | 65.3061 | 0.952677 | 0.358236 | 0.358236 | Cumple |
| 210 | Columna_1 | IR 610 x 113.1 | GRAV_DISEÑO | I | 848.4017 | 9.7584 | 11.9056 | 65.3061 | 0.952677 | 0.349391 | 0.349391 | Cumple |
| 207 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 504.9948 | 27.5012 | 29.7148 | 65.3061 | 0.952677 | 0.314068 | 0.314068 | Cumple |
| 104 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 283.3298 | 207.0140 | 4.0774 | 65.3061 | 0.952677 | 0.301614 | 0.301614 | Cumple |
| 101 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 306.1457 | 204.0346 | 3.7044 | 65.3061 | 0.952677 | 0.299347 | 0.299347 | Cumple |
| 406 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 181.3880 | 3.7801 | 38.7453 | 65.3061 | 0.952677 | 0.298934 | 0.298934 | Cumple |
| 105 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 522.6292 | 169.0625 | 4.1506 | 65.3061 | 0.952677 | 0.297802 | 0.297802 | Cumple |
| 306 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 415.3680 | 7.3765 | 31.0590 | 65.3061 | 0.952677 | 0.286933 | 0.286933 | Cumple |
| 307 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 318.5295 | 23.8190 | 29.4748 | 65.3061 | 0.952677 | 0.278995 | 0.278995 | Cumple |
| 108 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 393.1481 | 168.2124 | 4.3427 | 65.3061 | 0.952677 | 0.2778 | 0.2778 | Cumple |
| 407 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 137.4176 | 33.0490 | 31.5922 | 65.3061 | 0.952677 | 0.275245 | 0.275245 | Cumple |
| 203 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 351.5881 | 12.0732 | 29.6408 | 65.3061 | 0.952677 | 0.272345 | 0.272345 | Cumple |
| 410 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 172.5959 | 7.3221 | 32.7324 | 65.3061 | 0.952677 | 0.260157 | 0.260157 | Cumple |
| 202 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 370.6496 | 6.7221 | 28.1716 | 65.3061 | 0.952677 | 0.259333 | 0.259333 | Cumple |
| 109 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 512.6419 | 135.1083 | 4.2270 | 65.3061 | 0.952677 | 0.259194 | 0.259194 | Cumple |
| 310 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 393.7500 | 13.1696 | 25.3346 | 65.3061 | 0.952677 | 0.250609 | 0.250609 | Cumple |
| 303 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 217.6346 | 8.6768 | 29.4464 | 65.3061 | 0.952677 | 0.246167 | 0.246167 | Cumple |
| 302 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 230.9392 | 9.7603 | 27.4930 | 65.3061 | 0.952677 | 0.23604 | 0.23604 | Cumple |
| 211 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 357.7804 | 12.6886 | 23.8377 | 65.3061 | 0.952677 | 0.23413 | 0.23413 | Cumple |
| 112 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 250.9736 | 136.2242 | 3.5982 | 65.3061 | 0.952677 | 0.21492 | 0.21492 | Cumple |
| 103 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 480.0324 | 23.1189 | 16.4711 | 65.3061 | 0.952677 | 0.2143 | 0.2143 | Cumple |
| 102 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 505.0877 | 19.3735 | 16.0417 | 65.3061 | 0.952677 | 0.21115 | 0.21115 | Cumple |
| 114 | Columna_1 | IR 610 x 113.1 | GRAV_DISEÑO | I | 641.3535 | 6.6348 | 0.315588 | 65.3061 | 0.952677 | 0.210357 | 0.210357 | Cumple |
| 311 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 227.0683 | 6.9422 | 24.0771 | 65.3061 | 0.952677 | 0.208844 | 0.208844 | Cumple |
| 411 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | J | 93.9582 | 2.6394 | 26.7273 | 65.3061 | 0.952677 | 0.20134 | 0.20134 | Cumple |
| 403 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 94.2957 | 5.3908 | 25.9732 | 65.3061 | 0.952677 | 0.199256 | 0.199256 | Cumple |
| 113 | Columna_1 | IR 610 x 113.1 | SISMO_DOWN_1 | I | 297.2999 | 108.6470 | 3.9860 | 65.3061 | 0.952677 | 0.19437 | 0.19437 | Cumple |

#### Columna IR 106 - interaccion_pm

| Parametro | Valor |
| --- | --- |
| Perfil | IR 610 x 113.1 |
| Combinacion / extremo | GRAV_DISEÑO / J |
| Orientacion | My local = eje fuerte; Mz local = eje debil |
| Kx / Ky / Kz | 1.0 / 1.0 / 1.0 |
| KL/r x | 13.0081 |
| KL/r y | 65.3061 |
| Q local | 0.952677 |
| Estado axial | pandeo_flexion_y |
| PR (kN) | 3,176.51 |
| MR eje fuerte (kN-m) | 903.9805 |
| MR eje debil (kN-m) | 145.5492 |
| Control flexion fuerte | PLT |
| Control flexion debil | fluencia |
| Pu/PR | 0.381669 |
| Mux/MRx | 0.00238369 |
| Muy/MRy | 0.0666503 |
| Interaccion P-M | 0.443033 |
| Ecuacion | Pu/PR >= 0.20: Pu/PR + 8/9(Mux/MRx + Muy/MRy) |
| Utilizacion control | 0.443033 |
| Dictamen | Cumple |

#### Columna IR 106 - axial_maximo

| Parametro | Valor |
| --- | --- |
| Perfil | IR 610 x 113.1 |
| Combinacion / extremo | GRAV_DISEÑO / I |
| Orientacion | My local = eje fuerte; Mz local = eje debil |
| Kx / Ky / Kz | 1.0 / 1.0 / 1.0 |
| KL/r x | 13.0081 |
| KL/r y | 65.3061 |
| Q local | 0.952677 |
| Estado axial | pandeo_flexion_y |
| PR (kN) | 3,176.51 |
| MR eje fuerte (kN-m) | 903.9805 |
| MR eje debil (kN-m) | 145.5492 |
| Control flexion fuerte | PLT |
| Control flexion debil | fluencia |
| Pu/PR | 0.383122 |
| Mux/MRx | 0.00580567 |
| Muy/MRy | 0.0326046 |
| Interaccion P-M | 0.417265 |
| Ecuacion | Pu/PR >= 0.20: Pu/PR + 8/9(Mux/MRx + Muy/MRy) |
| Utilizacion control | 0.417265 |
| Dictamen | Cumple |

#### Columna IR 104 - momento_maximo

| Parametro | Valor |
| --- | --- |
| Perfil | IR 610 x 113.1 |
| Combinacion / extremo | SISMO_DOWN_1 / I |
| Orientacion | My local = eje fuerte; Mz local = eje debil |
| Kx / Ky / Kz | 1.0 / 1.0 / 1.0 |
| KL/r x | 13.0081 |
| KL/r y | 65.3061 |
| Q local | 0.952677 |
| Estado axial | pandeo_flexion_y |
| PR (kN) | 3,176.51 |
| MR eje fuerte (kN-m) | 903.9805 |
| MR eje debil (kN-m) | 145.5492 |
| Control flexion fuerte | PLT |
| Control flexion debil | fluencia |
| Pu/PR | 0.0891954 |
| Mux/MRx | 0.229003 |
| Muy/MRy | 0.028014 |
| Interaccion P-M | 0.301614 |
| Ecuacion | Pu/PR < 0.20: Pu/(2PR) + Mux/MRx + Muy/MRy |
| Utilizacion control | 0.301614 |
| Dictamen | Cumple |

Alertas:

- Al menos una columna IR contiene elementos esbeltos en compresion; se aplico el factor Q de pandeo local.

### 9.5 Diseno Detallado de Columnas Criticas OR

No se encontraron columnas OR simples para esta revision.

### 9.6 Diseno Axial LRFD de Contravientos OR

No se encontraron contravientos OR para revisar axialmente.

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 426 | Viga_2 | IR 254 x 38.7 | GRAV_DISEÑO | 56,320.88 | 159,204.09 | 0.353765 | Cumple |
| 427 | Viga_2 | IR 254 x 38.7 | GRAV_DISEÑO | 56,274.62 | 159,204.09 | 0.353475 | Cumple |
| 432 | Viga_2 | IR 254 x 38.7 | GRAV_DISEÑO | 48,137.19 | 159,204.09 | 0.302362 | Cumple |
| 433 | Viga_2 | IR 254 x 38.7 | GRAV_DISEÑO | 48,123.77 | 159,204.09 | 0.302277 | Cumple |
| 221 | Viga_1 | IR 457 x 59.5 | SISMO_DOWN_1 | 90,734.17 | 398,786.06 | 0.227526 | Cumple |
| 321 | Viga_1 | IR 457 x 59.5 | SISMO_DOWN_1 | 84,758.09 | 398,786.06 | 0.21254 | Cumple |
| 224 | Viga_1 | IR 457 x 59.5 | SISMO_DOWN_1 | 82,598.57 | 398,786.06 | 0.207125 | Cumple |
| 229 | Viga_1 | IR 457 x 59.5 | SISMO_DOWN_1 | 81,510.56 | 398,786.06 | 0.204397 | Cumple |
| 424 | Viga_2 | IR 254 x 38.7 | SISMO_DOWN_1 | 31,384.33 | 159,204.09 | 0.197133 | Cumple |
| 226 | Viga_1 | IR 457 x 59.5 | GRAV_DISEÑO | 77,662.51 | 398,786.06 | 0.194747 | Cumple |
| 227 | Viga_1 | IR 457 x 59.5 | GRAV_DISEÑO | 77,632.69 | 398,786.06 | 0.194673 | Cumple |
| 121 | Viga_1 | IR 457 x 59.5 | SISMO_DOWN_1 | 77,620.69 | 398,786.06 | 0.194642 | Cumple |

### 10.2 Contravientos Criticos

_Sin datos disponibles._

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
