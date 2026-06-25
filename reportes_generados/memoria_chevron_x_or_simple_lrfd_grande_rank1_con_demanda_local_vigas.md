# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-06-09 11:18:59

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 132 |
| Elementos | 280 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 37,333.04 |
| Masa sismica (kg) | 605,654.77 |

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
| Masa propia elementos (kg) | 37,333.04 |
| Masa sismica total (kg) | 605,654.77 |
| Peso sismico total (N) | 5,941,473.29 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 2,418.37 | 2,418.37 |
| 3.2000 | 122,954.22 | 19,126.21 | 9,482.60 | 151,563.03 |
| 6.4000 | 122,954.22 | 19,126.21 | 9,333.26 | 151,413.69 |
| 9.6000 | 122,954.22 | 19,126.21 | 9,183.92 | 151,264.35 |
| 12.8000 | 122,954.22 | 19,126.21 | 6,914.89 | 148,995.32 |

## 5. Analisis Modal

El numero de modos se selecciona buscando alcanzar la participacion modal minima configurada.

| Parametro | Valor |
| --- | --- |
| Objetivo participacion | 0.9 |
| Modos usados | 9 |
| Maximo modos |  |

### 5.1 Periodos

| Modo | Periodo (s) |
| --- | --- |
| 1 | 0.43315 |
| 2 | 0.426314 |
| 3 | 0.270808 |
| 4 | 0.19954 |
| 5 | 0.196855 |
| 6 | 0.196108 |
| 7 | 0.195336 |
| 8 | 0.148652 |
| 9 | 0.148166 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.803355 |
| 2 |  |  | 0.803543 |
| 3 |  |  | 0.803603 |
| 4 |  |  | 0.803749 |
| 5 |  |  | 0.803868 |
| 6 |  |  | 0.80397 |
| 7 |  |  | 0.803973 |
| 8 |  |  | 0.959711 |
| 9 |  |  | 0.959711 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.000182147 |
| 2 |  |  | 0.802959 |
| 3 |  |  | 0.804353 |
| 4 |  |  | 0.804355 |
| 5 |  |  | 0.804357 |
| 6 |  |  | 0.804361 |
| 7 |  |  | 0.804361 |
| 8 |  |  | 0.804361 |
| 9 |  |  | 0.951154 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.683463 | 1.0000 | Cumple |
| Deriva servicio | 0.000820255 | 0.005 | Cumple |
| Deriva colapso | 0.00364558 | 0.01 | Cumple |
| Desp. azotea | 0.00919131 | 0.1 | Cumple |
| KL/r | 40.6298 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000568926 | 0.00252856 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.000811836 | 0.00360816 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.000820255 | 0.00364558 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.000671267 | 0.00298341 |

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
| Ratio max | 0.337416 |
| Flecha maxima (mm) | 8.5760 |
| Flecha admisible (mm) | 25.4167 |
| Estado global | Cumple |

#### Claros Criticos

| Claro | Nivel (m) | Grupo | Perfil | Elementos | L (m) | delta max (mm) | delta adm (mm) | Ratio | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DV-0036 | 3.2000 | Viga_1 | IR 203 x 31.3 | 149, 150 | 6.1000 | 8.5760 | 25.4167 | 0.337416 | Cumple |
| DV-0062 | 6.4000 | Viga_1 | IR 203 x 31.3 | 249, 250 | 6.1000 | 8.5718 | 25.4167 | 0.337252 | Cumple |
| DV-0088 | 9.6000 | Viga_1 | IR 203 x 31.3 | 349, 350 | 6.1000 | 8.5677 | 25.4167 | 0.33709 | Cumple |
| DV-0010 | 12.8000 | Viga_2 | IR 203 x 31.3 | 449, 450 | 6.1000 | 8.5617 | 25.4167 | 0.336853 | Cumple |
| DV-0024 | 12.8000 | Viga_2 | IR 203 x 31.3 | 430, 431 | 5.3000 | 5.5823 | 22.0833 | 0.252784 | Cumple |
| DV-0050 | 3.2000 | Viga_1 | IR 203 x 31.3 | 130, 131 | 5.3000 | 5.1497 | 22.0833 | 0.233194 | Cumple |
| DV-0076 | 6.4000 | Viga_1 | IR 203 x 31.3 | 230, 231 | 5.3000 | 5.1040 | 22.0833 | 0.231126 | Cumple |
| DV-0102 | 9.6000 | Viga_1 | IR 203 x 31.3 | 330, 331 | 5.3000 | 4.9968 | 22.0833 | 0.226268 | Cumple |
| DV-0018 | 12.8000 | Viga_2 | IR 203 x 31.3 | 437, 438 | 5.3000 | 4.7449 | 22.0833 | 0.214862 | Cumple |
| DV-0044 | 3.2000 | Viga_1 | IR 203 x 31.3 | 137, 138 | 5.3000 | 4.3589 | 22.0833 | 0.197386 | Cumple |
| DV-0070 | 6.4000 | Viga_1 | IR 203 x 31.3 | 237, 238 | 5.3000 | 4.3326 | 22.0833 | 0.196195 | Cumple |
| DV-0096 | 9.6000 | Viga_1 | IR 203 x 31.3 | 337, 338 | 5.3000 | 4.2518 | 22.0833 | 0.192534 | Cumple |

#### Alertas

- 56 claros no tienen nodos intermedios; la flecha local puede estar subestimada.

Nota: en claros sin nodos intermedios el indicador nodal puede subestimar la flecha maxima local; esos casos deben refinarse si controlan la revision.

### 8.5 Fuerzas Internas para Diseno LRFD

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
| M max (kN-m) | 71.0553 |
| Combo momento critico | GRAV_DISEÑO |
| Elemento axial critico | 106 |
| P max (kN) | 1,033.05 |
| Combo axial critico | GRAV_DISEÑO |
| Grupo momento critico | Viga_1 |
| Grupo M max (kN-m) | 71.0553 |
| Grupo axial critico | Columna_2 |
| Grupo P max (kN) | 1,033.05 |

#### Envolvente por Grupo de Diseno

| Grupo | Tipo | Perfiles | Elems | P max (kN) | Elem P | Combo P | V max (kN) | Elem V | Combo V | M max (kN-m) | Elem M | Combo M |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Columna_1 | COLUMNA | ORC 203 x 9.5 | 48 | 542.6389 | 102 | SISMO_DOWN_1 | 8.4372 | 408 | GRAV_DISEÑO | 15.7823 | 408 | GRAV_DISEÑO |
| Columna_2 | COLUMNA | ORC 305 x 12.7 | 16 | 1,033.05 | 106 | GRAV_DISEÑO | 32.3666 | 406 | GRAV_DISEÑO | 61.9157 | 406 | GRAV_DISEÑO |
| Contraviento_1 | CONTRAVIENTO | ORC 114 x 5.3 | 32 | 116.6001 | 156 | SISMO_DOWN_1 | 125.7709 | 257 | SISMO_DOWN_1 | 0 | 0 |  |
| Contraviento_2 | CONTRAVIENTO | ORC 89 x 4.8 | 32 | 77.4511 | 367 | SISMO_DOWN_1 | 85.1076 | 358 | SISMO_DOWN_1 | 0 | 0 |  |
| Viga_1 | VIGA | IR 203 x 31.3 | 114 | 1.47751e-14 | 252 | SISMO_DOWN_1 | 76.1855 | 130 | GRAV_DISEÑO | 71.0553 | 330 | GRAV_DISEÑO |
| Viga_2 | VIGA | IR 203 x 31.3 | 38 | 5.76664e-14 | 452 | SISMO_DOWN_5 | 75.4495 | 430 | GRAV_DISEÑO | 67.8248 | 430 | GRAV_DISEÑO |

#### Envolvente Critica por Momento Resultante

| Elem | Tipo | Grupo | Perfil | M max (kN-m) | Combo M | Extremo M | P max (kN) | Combo P | V max (kN) | Combo V |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 330 | VIGA | Viga_1 | IR 203 x 31.3 | 71.0553 | GRAV_DISEÑO | I | 0 |  | 76.1218 | GRAV_DISEÑO |
| 130 | VIGA | Viga_1 | IR 203 x 31.3 | 70.7319 | GRAV_DISEÑO | I | 0 |  | 76.1855 | GRAV_DISEÑO |
| 230 | VIGA | Viga_1 | IR 203 x 31.3 | 70.7199 | GRAV_DISEÑO | I | 0 |  | 76.1169 | GRAV_DISEÑO |
| 430 | VIGA | Viga_2 | IR 203 x 31.3 | 67.8248 | GRAV_DISEÑO | I | 0 |  | 75.4495 | GRAV_DISEÑO |
| 406 | COLUMNA | Columna_2 | ORC 305 x 12.7 | 61.9157 | GRAV_DISEÑO | J | 258.1732 | GRAV_DISEÑO | 32.3666 | GRAV_DISEÑO |
| 137 | VIGA | Viga_1 | IR 203 x 31.3 | 59.3196 | GRAV_DISEÑO | I | 0 |  | 64.1216 | GRAV_DISEÑO |
| 337 | VIGA | Viga_1 | IR 203 x 31.3 | 59.1787 | GRAV_DISEÑO | I | 0 |  | 63.9177 | GRAV_DISEÑO |
| 237 | VIGA | Viga_1 | IR 203 x 31.3 | 59.0778 | GRAV_DISEÑO | I | 0 |  | 63.9794 | GRAV_DISEÑO |
| 437 | VIGA | Viga_2 | IR 203 x 31.3 | 56.4663 | GRAV_DISEÑO | I | 0 |  | 63.3588 | GRAV_DISEÑO |
| 410 | COLUMNA | Columna_2 | ORC 305 x 12.7 | 51.3522 | GRAV_DISEÑO | J | 246.0557 | GRAV_DISEÑO | 26.8656 | GRAV_DISEÑO |
| 407 | COLUMNA | Columna_2 | ORC 305 x 12.7 | 49.5374 | GRAV_DISEÑO | J | 188.1250 | GRAV_DISEÑO | 25.6162 | GRAV_DISEÑO |
| 452 | VIGA | Viga_2 | IR 203 x 31.3 | 45.4799 | GRAV_DISEÑO | I | 5.76664e-14 | SISMO_DOWN_5 | 30.6240 | GRAV_DISEÑO |

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

### 8.6 Diseno LRFD por Flexion de Vigas IR

La revision toma la mayor demanda entre la envolvente LRFD del modelo global y una revision gravitacional local por claro fisico. La demanda local redistribuye las cargas nodales tributarias como una carga distribuida equivalente sobre la viga, evitando que los tramos largos sin nodos intermedios queden subestimados en flexion o flecha. Para vigas IR se revisa la flexion alrededor del eje mayor `x-x` IMCA. Se usa `Cb = 1.0` y `Lb` igual a la longitud entre nodos del elemento, salvo configuracion explicita.

#### Resumen General

| Parametro | Valor |
| --- | --- |
| Vigas IR revisadas | 152 |
| Cumplen | 104 |
| No cumplen | 48 |
| Vigas controladas por demanda local | 144 |
| Elemento critico | 132 |
| Grupo critico | Viga_1 |
| Perfil critico | IR 203 x 31.3 |
| Combo critico | GRAV_DISEÑO |
| Demanda control | local_gravitacional |
| Mu max (kN-m) | 67.9592 |
| Mu modelo (kN-m) | 1.4598 |
| Mu local (kN-m) | 67.9592 |
| MR control (kN-m) | 44.6164 |
| Utilizacion flexion max | 1.5232 |
| Utilizacion control max | 1.5546 |
| Estado limite control | PLT |

#### Calculo Detallado de la Viga Critica

| Paso / Propiedad | Valor |
| --- | --- |
| 1. Perfil | IR 203 x 31.3 |
| 2. Eje de flexion | x-x IMCA / eje fuerte |
| Elemento | 132 |
| Grupo | Viga_1 |
| Combinacion Mu | GRAV_DISEÑO |
| Extremo Mu | VL-0034 |
| Demanda control | local_gravitacional |
| Mu modelo (kN-m) | 1.4598 |
| Mu local (kN-m) | 67.9592 |
| Claro local | VL-0034 |
| Longitud claro local (m) | 6.1000 |
| w diseno local (kN/m) | 14.6109 |
| Material | A992 |
| Fy (kg/cm2) | 3,515.00 |
| E (kg/cm2) | 2,039,696.53 |
| d (mm) | 211.0000 |
| bf (mm) | 134.0000 |
| tf (mm) | 10.2000 |
| tw (mm) | 6.4000 |
| h (mm) | 175.0000 |
| Ix (cm4) | 3,135.00 |
| Iy (cm4) | 407.0000 |
| Sx (cm3) | 299.0000 |
| Zx (cm3) | 335.0000 |
| ry (cm) | 3.2000 |
| J (cm4) | 12.0000 |
| Cw (cm6) | 40,817.00 |
| lambda_f = bf / 2tf | 6.5686 |
| lambda_f limite compacto | 9.1523 |
| lambda_f limite no compacto | 24.0850 |
| Clasificacion patin | compacta |
| lambda_w = h / tw | 27.3437 |
| lambda_w limite compacto | 59.0082 |
| lambda_w limite no compacto | 134.8758 |
| Clasificacion alma | compacta |
| 6. Inciso NTC seleccionado | 7.3 |
| Lb (m) | 6.1000 |
| Cb | 1.0000 |
| Lu (m) | 1.3565 |
| Lr (m) | 4.5118 |
| Mu (kN-m) | 67.9592 |
| Vu (kN) | 44.5634 |

#### Estados Limite de Flexion

| Estado | Nombre | Inciso | Aplica | Mn (kN-m) | MR (kN-m) | Util | Resultado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| F | Fluencia | NTC-Acero 7.3.1 / 7.4.2 | Si | 115.5152 | 103.9637 | 0.653682 | Cumple |
| PLT | Pandeo lateral por flexotorsion | NTC-Acero 7.3.2 / 7.4.3 | Si | 49.5738 | 44.6164 | 1.5232 | No cumple |
| PLP | Pandeo local del patin comprimido | NTC-Acero 7.4.4 | No | 115.5152 | 103.9637 | 0.653682 | Cumple |
| PLA | Pandeo local del alma por flexion | NTC-Acero 7.5 | No | 115.5152 | 103.9637 | 0.653682 | Cumple |

#### Verificacion Resistente

| Revision | Valor |
| --- | --- |
| 8. Mn control (kN-m) | 49.5738 |
| Estado limite control | PLT |
| 9. MR = FR Mn (kN-m) | 44.6164 |
| Mu modelo (kN-m) | 1.4598 |
| Mu local (kN-m) | 67.9592 |
| 10. Mu / MR | 1.5232 |
| Vn (kN) | 279.3882 |
| 11. VR (kN) | 251.4494 |
| Vu / VR | 0.177226 |
| 12. Mu/MR + (Vu/VR)^2 | 1.5546 |
| Estado final | No cumple |

#### Referencia de Servicio

| Parametro | Valor |
| --- | --- |
| 13. Claro de deflexion asociado | VL-0034 |
| Ratio flecha | 1.2377 |
| Delta max (mm) | 31.4572 |
| Delta adm (mm) | 25.4167 |
| Estado flecha | No cumple |
| Flecha local calculada (mm) | 31.4572 |
| Flecha local admisible (mm) | 25.4167 |
| Ratio flecha local | 1.2377 |

#### Resumen de Diseno de Vigas IR

| Elem | Grupo | Perfil | Inciso | Demanda | Mu (kN-m) | Mu modelo | Mu local | MR (kN-m) | Vu (kN) | VR (kN) | Flexion | Cortante | F+V | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 132 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 67.9592 | 1.4598 | 67.9592 | 44.6164 | 44.5634 | 251.4494 | 1.5232 | 0.177226 | 1.5546 | No cumple |
| 232 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 67.9592 | 1.5977 | 67.9592 | 44.6164 | 44.5634 | 251.4494 | 1.5232 | 0.177226 | 1.5546 | No cumple |
| 332 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 67.9592 | 1.6980 | 67.9592 | 44.6164 | 44.5634 | 251.4494 | 1.5232 | 0.177226 | 1.5546 | No cumple |
| 432 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 67.9592 | 1.7507 | 67.9592 | 44.6164 | 44.5634 | 251.4494 | 1.5232 | 0.177226 | 1.5546 | No cumple |
| 134 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 6.7819 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 234 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 6.4637 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 334 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 5.6822 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 434 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 6.4982 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 128 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 7.1037 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 228 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 6.7535 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 328 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 5.8949 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 428 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 63.9111 | 7.1155 | 63.9111 | 42.7822 | 40.4501 | 251.4494 | 1.4939 | 0.160868 | 1.5198 | No cumple |
| 122 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 1.5739 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 222 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 1.9195 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 322 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 2.0447 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 422 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 2.0932 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 139 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 1.4352 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 239 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 1.8978 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 339 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 2.1042 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 439 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 60.6726 | 1.8250 | 60.6726 | 43.7630 | 39.1436 | 251.4494 | 1.3864 | 0.155672 | 1.4106 | No cumple |
| 129 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 55.2796 | 1.2466 | 55.2796 | 44.6164 | 36.2490 | 251.4494 | 1.2390 | 0.14416 | 1.2598 | No cumple |
| 229 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 55.2796 | 1.4785 | 55.2796 | 44.6164 | 36.2490 | 251.4494 | 1.2390 | 0.14416 | 1.2598 | No cumple |
| 329 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 55.2796 | 1.3807 | 55.2796 | 44.6164 | 36.2490 | 251.4494 | 1.2390 | 0.14416 | 1.2598 | No cumple |
| 429 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 55.2796 | 1.4777 | 55.2796 | 44.6164 | 36.2490 | 251.4494 | 1.2390 | 0.14416 | 1.2598 | No cumple |
| 230 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 75.7506 | 70.7199 | 75.7506 | 102.1892 | 76.1169 | 251.4494 | 0.741278 | 0.302713 | 0.832913 | No cumple |
| 231 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 75.7506 | 42.9594 | 75.7506 | 73.7541 | 57.1703 | 251.4494 | 1.0271 | 0.227363 | 1.0788 | No cumple |
| 330 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 75.7506 | 71.0553 | 75.7506 | 102.1892 | 76.1218 | 251.4494 | 0.741278 | 0.302732 | 0.832925 | No cumple |
| 331 | Viga_1 | IR 203 x 31.3 | 7.3 | local_gravitacional | 75.7506 | 42.6317 | 75.7506 | 73.7541 | 57.1703 | 251.4494 | 1.0271 | 0.227363 | 1.0788 | No cumple |
| 430 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 75.7506 | 67.8248 | 75.7506 | 102.1892 | 75.4495 | 251.4494 | 0.741278 | 0.300058 | 0.831313 | No cumple |
| 431 | Viga_2 | IR 203 x 31.3 | 7.3 | local_gravitacional | 75.7506 | 44.8527 | 75.7506 | 73.7541 | 57.1703 | 251.4494 | 1.0271 | 0.227363 | 1.0788 | No cumple |

#### Alertas

Demanda controlada por revision gravitacional local de claro fisico.

- PLT controla al menos una viga IR con Cb=1.0 y Lb entre nodos; confirmar arriostramiento lateral real del patin comprimido antes de dictaminar el diseno final.
- 56 claros sin nodos intermedios: la revision local usa formulas cerradas de viga simplemente apoyada para capturar momento y flecha maxima.
- Existen vigas IR que no cumplen bajo el criterio LRFD configurado.

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
| Perfil | ORC 305 x 12.7 |
| Combinacion critica | GRAV_DISEÑO |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.013484 |
| Ix (m4) | 0.000190218 |
| Iy (m4) | 0.000190218 |
| r_min (m) | 0.118773 |
| KL/r | 26.9422 |
| Pu (kN) | 32.3749 |
| Mu (kN-m) | 61.9108 |
| My local (kN-m) | 61.9107 |
| Mz local (kN-m) | 0.0677276 |
| phi Pn (kN) | 3,667.70 |
| phi Mny (kN-m) | 419.3731 |
| phi Mnz (kN-m) | 419.3731 |
| Extremo critico | j |
| Tipo revision | OR biaxial simetrico: misma capacidad en ambos ejes locales |

Sustitucion numerica:

- `Pu/phiPn = 32.375 / 3667.700 = 0.0088`
- `My/phiMny = 61.911 / 419.373 = 0.1476`
- `Mz/phiMnz = 0.068 / 419.373 = 0.0002`
- `Suma flexion biaxial = 0.1478`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`
- `D/C = 0.1522`
- Estado: **Cumple**

Nota: esta seccion resume la revision global historica del framework. Para columnas OR simples, el dictamen detallado de la seccion 9.4 debe usarse como criterio gobernante de memoria.

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | My (N-m) | Mz (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 406 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | 26.9422 | 32,374.95 | 61,910.75 | 67.7276 | 0.152202 | Cumple |
| 410 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | 26.9422 | 26,867.48 | 51,345.66 | 67.7276 | 0.126259 | Cumple |
| 106 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 19,225.57 | 44,815.25 | 20.1373 | 0.109531 | Cumple |
| 110 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 17,446.66 | 40,971.92 | 20.1373 | 0.100124 | Cumple |
| 206 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | 26.9422 | 23,691.72 | 38,950.88 | 43.7365 | 0.0962129 | Cumple |
| 210 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | 26.9422 | 19,815.14 | 32,657.47 | 43.7365 | 0.0806777 | Cumple |
| 107 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 13,475.22 | 32,003.44 | 20.1373 | 0.0781976 | Cumple |
| 306 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 17,976.67 | 31,227.05 | 51.4341 | 0.0770346 | Cumple |
| 111 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 12,822.86 | 30,630.50 | 20.1373 | 0.0748349 | Cumple |
| 407 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | 26.9422 | 14,797.89 | 28,346.83 | 67.7276 | 0.0697722 | Cumple |
| 310 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 15,565.58 | 27,124.12 | 51.4341 | 0.0669224 | Cumple |
| 411 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 12,336.88 | 25,055.38 | 64.3164 | 0.06158 | Cumple |
| 103 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | 40.6298 | 3,326.76 | 6,617.55 | 4.4258 | 0.0490909 | Cumple |
| 115 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | 40.6298 | 3,265.78 | 6,527.12 | 4.4258 | 0.048416 | Cumple |
| 102 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | 40.6298 | 3,026.14 | 6,459.91 | 4.4258 | 0.0478577 | Cumple |
| 114 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | 40.6298 | 2,956.50 | 6,291.94 | 4.4258 | 0.0466168 | Cumple |
| 207 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 10,695.70 | 18,245.55 | 49.7392 | 0.0450834 | Cumple |
| 307 | Columna_2 | ORC 305 x 12.7 | SISMO_DOWN_1 | 26.9422 | 9,950.35 | 17,726.76 | 51.4341 | 0.0437488 | Cumple |
| 112 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | 40.6298 | 2,543.60 | 5,698.96 | 4.4258 | 0.0421873 | Cumple |
| 108 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | 40.6298 | 2,536.46 | 5,686.48 | 4.4258 | 0.0420945 | Cumple |

### 9.4 Diseno Detallado de Columnas Criticas OR

Esta revision aplica al alcance actual de columnas OR simples simetricas. Las columnas IR y OR rellenas quedan preparadas para implementarse como verificadores adicionales. Para la interaccion axial-flexion biaxial se usan demandas concurrentes por combinacion y extremo; no se mezclan maximos independientes de la envolvente.

#### 9.4.1 Identificacion de Columnas Criticas

| Parametro | Valor |
| --- | --- |
| Columnas OR revisadas | 64 |
| Cumplen | 64 |
| No cumplen | 0 |
| Elemento critico por interaccion | 102 |
| Interaccion P-M maxima | 0.357784 |
| Elemento axial max | 106 |
| Pu max (kN) | 1,033.05 |
| Elemento momento max | 406 |
| M max (kN-m) | 61.9157 |
| Criticas unicas | 3 |

| Rol | Elem | Grupo | Perfil | Combo | Ext | Pu (kN) | M (kN-m) | Interaccion | Dictamen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| interaccion_pm | 102 | Columna_1 | ORC 203 x 9.5 | SISMO_DOWN_1 | I | 542.6389 | 6.1857 | 0.357784 | Cumple |
| axial_maximo | 106 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | I | 1,033.05 | 13.5563 | 0.310614 | Cumple |
| momento_maximo | 406 | Columna_2 | ORC 305 x 12.7 | GRAV_DISEÑO | J | 253.5601 | 61.9157 | 0.184063 | Cumple |

#### Columna 102 - interaccion_pm

| Parametro | Valor |
| --- | --- |
| 9.4.1 Rol critico | interaccion_pm |
| Elemento | 102 |
| Grupo | Columna_1 |
| Perfil | ORC 203 x 9.5 |
| Combinacion | SISMO_DOWN_1 |
| Extremo | I |
| 9.4.2 Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| E (kg/cm2) | 2,039,696.53 |
| A (cm2) | 67.1000 |
| Ix = Iy (cm4) | 4,162.30 |
| rx = ry (cm) | 7.8700 |
| J (cm4) | 6,659.70 |
| Dimension exterior (mm) | 203.0000 |
| Ancho plano b (mm) | 177.1100 |
| Espesor t (mm) | 8.9000 |
| 9.4.3 b/t | 19.9000 |
| lambda compacto | 28.1183 |
| lambda no compacto | 35.1479 |
| Clasificacion local | compacta |
| Tipo elemento local | pared atiesada en compresion |
| 9.4.4 Kx | 1.0000 |
| Ky | 1.0000 |
| L (m) | 3.2000 |
| KL/r x | 40.6607 |
| KL/r y | 40.6607 |
| Fcr x (MPa) | 283.9442 |
| Fcr y (MPa) | 283.9442 |
| Ae (cm2) | 67.1000 |
| Q local | 1.0000 |
| Pn control (kN) | 1,905.27 |
| PR (kN) | 1,714.74 |
| Estado limite axial | pandeo_x |
| 9.4.5 S (cm3) | 408.0000 |
| Z (cm3) | 481.8000 |
| MRx (kN-m) | 137.6108 |
| MRy (kN-m) | 137.6108 |
| Estado limite flexion | fluencia_plastica |
| 9.4.6 Vy max (kN) | 3.1006 |
| Vz max (kN) | 0.247126 |
| VRy = VRz (kN) | 619.2316 |
| Cv cortante | 1.0000 |
| 9.4.7 Pu concurrente (kN) | 542.6389 |
| Mux concurrente (kN-m) | 0.216115 |
| Muy concurrente (kN-m) | 6.1820 |
| Pu/PR | 0.316456 |
| Mux/MRx | 0.00157048 |
| Muy/MRy | 0.0449235 |
| Interaccion P-M | 0.357784 |
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
| FG | Fluencia general | NTC-Acero 6.3.1 | Si | 1,916,497.79 | 0.283141 | Cumple |
| PX | Pandeo por flexion eje x | NTC-Acero 6.3.1 | Si | 1,714,738.95 | 0.316456 | Cumple |
| PY | Pandeo por flexion eje y | NTC-Acero 6.3.1 | Si | 1,714,738.95 | 0.316456 | Cumple |
| PT | Pandeo torsional/flexotorsional | NTC-Acero 6.3.2 | No | 1,714,738.95 | 0.316456 | Cumple |
| MX | Flexion eje x | NTC-Acero 7 | Si | 137,610.82 | 0.00157048 | Cumple |
| MY | Flexion eje y | NTC-Acero 7 | Si | 137,610.82 | 0.0449235 | Cumple |
| VY | Cortante local y | NTC-Acero 7.10 | Si | 619,231.57 | 0.00500714 | Cumple |
| VZ | Cortante local z | NTC-Acero 7.10 | Si | 619,231.57 | 0.000399084 | Cumple |

#### Columna 106 - axial_maximo

| Parametro | Valor |
| --- | --- |
| 9.4.1 Rol critico | axial_maximo |
| Elemento | 106 |
| Grupo | Columna_2 |
| Perfil | ORC 305 x 12.7 |
| Combinacion | GRAV_DISEÑO |
| Extremo | I |
| 9.4.2 Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| E (kg/cm2) | 2,039,696.53 |
| A (cm2) | 134.8400 |
| Ix = Iy (cm4) | 19,021.80 |
| rx = ry (cm) | 11.8900 |
| J (cm4) | 30,301.60 |
| Dimension exterior (mm) | 305.0000 |
| Ancho plano b (mm) | 269.0400 |
| Espesor t (mm) | 11.8000 |
| 9.4.3 b/t | 22.8000 |
| lambda compacto | 28.1183 |
| lambda no compacto | 35.1479 |
| Clasificacion local | compacta |
| Tipo elemento local | pared atiesada en compresion |
| 9.4.4 Kx | 1.0000 |
| Ky | 1.0000 |
| L (m) | 3.2000 |
| KL/r x | 26.9134 |
| KL/r y | 26.9134 |
| Fcr x (MPa) | 302.2581 |
| Fcr y (MPa) | 302.2581 |
| Ae (cm2) | 134.8400 |
| Q local | 1.0000 |
| Pn control (kN) | 4,075.65 |
| PR (kN) | 3,668.08 |
| Estado limite axial | pandeo_x |
| 9.4.5 S (cm3) | 1,248.70 |
| Z (cm3) | 1,468.30 |
| MRx (kN-m) | 419.3731 |
| MRy (kN-m) | 419.3731 |
| Estado limite flexion | fluencia_plastica |
| 9.4.6 Vy max (kN) | 19.2169 |
| Vz max (kN) | 0.126714 |
| VRy = VRz (kN) | 1,233.53 |
| Cv cortante | 1.0000 |
| 9.4.7 Pu concurrente (kN) | 1,033.05 |
| Mux concurrente (kN-m) | 0.117789 |
| Muy concurrente (kN-m) | 13.5557 |
| Pu/PR | 0.281632 |
| Mux/MRx | 0.000280869 |
| Muy/MRy | 0.0323238 |
| Interaccion P-M | 0.310614 |
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
| FG | Fluencia general | NTC-Acero 6.3.1 | Si | 3,851,275.13 | 0.268236 | Cumple |
| PX | Pandeo por flexion eje x | NTC-Acero 6.3.1 | Si | 3,668,083.30 | 0.281632 | Cumple |
| PY | Pandeo por flexion eje y | NTC-Acero 6.3.1 | Si | 3,668,083.30 | 0.281632 | Cumple |
| PT | Pandeo torsional/flexotorsional | NTC-Acero 6.3.2 | No | 3,668,083.30 | 0.281632 | Cumple |
| MX | Flexion eje x | NTC-Acero 7 | Si | 419,373.13 | 0.000280869 | Cumple |
| MY | Flexion eje y | NTC-Acero 7 | Si | 419,373.13 | 0.0323238 | Cumple |
| VY | Cortante local y | NTC-Acero 7.10 | Si | 1,233,527.67 | 0.0155788 | Cumple |
| VZ | Cortante local z | NTC-Acero 7.10 | Si | 1,233,527.67 | 0.000102725 | Cumple |

#### Columna 406 - momento_maximo

| Parametro | Valor |
| --- | --- |
| 9.4.1 Rol critico | momento_maximo |
| Elemento | 406 |
| Grupo | Columna_2 |
| Perfil | ORC 305 x 12.7 |
| Combinacion | GRAV_DISEÑO |
| Extremo | J |
| 9.4.2 Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| E (kg/cm2) | 2,039,696.53 |
| A (cm2) | 134.8400 |
| Ix = Iy (cm4) | 19,021.80 |
| rx = ry (cm) | 11.8900 |
| J (cm4) | 30,301.60 |
| Dimension exterior (mm) | 305.0000 |
| Ancho plano b (mm) | 269.0400 |
| Espesor t (mm) | 11.8000 |
| 9.4.3 b/t | 22.8000 |
| lambda compacto | 28.1183 |
| lambda no compacto | 35.1479 |
| Clasificacion local | compacta |
| Tipo elemento local | pared atiesada en compresion |
| 9.4.4 Kx | 1.0000 |
| Ky | 1.0000 |
| L (m) | 3.2000 |
| KL/r x | 26.9134 |
| KL/r y | 26.9134 |
| Fcr x (MPa) | 302.2581 |
| Fcr y (MPa) | 302.2581 |
| Ae (cm2) | 134.8400 |
| Q local | 1.0000 |
| Pn control (kN) | 4,075.65 |
| PR (kN) | 3,668.08 |
| Estado limite axial | pandeo_x |
| 9.4.5 S (cm3) | 1,248.70 |
| Z (cm3) | 1,468.30 |
| MRx (kN-m) | 419.3731 |
| MRy (kN-m) | 419.3731 |
| Estado limite flexion | fluencia_plastica |
| 9.4.6 Vy max (kN) | 32.3623 |
| Vz max (kN) | 0.808481 |
| VRy = VRz (kN) | 1,233.53 |
| Cv cortante | 1.0000 |
| 9.4.7 Pu concurrente (kN) | 253.5601 |
| Mux concurrente (kN-m) | 0.78562 |
| Muy concurrente (kN-m) | 61.9107 |
| Pu/PR | 0.069126 |
| Mux/MRx | 0.00187332 |
| Muy/MRy | 0.147627 |
| Interaccion P-M | 0.184063 |
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
| FG | Fluencia general | NTC-Acero 6.3.1 | Si | 3,851,275.13 | 0.065838 | Cumple |
| PX | Pandeo por flexion eje x | NTC-Acero 6.3.1 | Si | 3,668,083.30 | 0.069126 | Cumple |
| PY | Pandeo por flexion eje y | NTC-Acero 6.3.1 | Si | 3,668,083.30 | 0.069126 | Cumple |
| PT | Pandeo torsional/flexotorsional | NTC-Acero 6.3.2 | No | 3,668,083.30 | 0.069126 | Cumple |
| MX | Flexion eje x | NTC-Acero 7 | Si | 419,373.13 | 0.00187332 | Cumple |
| MY | Flexion eje y | NTC-Acero 7 | Si | 419,373.13 | 0.147627 | Cumple |
| VY | Cortante local y | NTC-Acero 7.10 | Si | 1,233,527.67 | 0.0262356 | Cumple |
| VZ | Cortante local z | NTC-Acero 7.10 | Si | 1,233,527.67 | 0.000655422 | Cumple |

#### Resumen de Columnas OR

| Elem | Grupo | Perfil | Pu (kN) | M (kN-m) | KL/r | P-M | Vy | Vz | Control | Dictamen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 102 | Columna_1 | ORC 203 x 9.5 | 542.6389 | 6.1857 | 40.6607 | 0.357784 | 0.00500714 | 0.000399084 | 0.357784 | Cumple |
| 114 | Columna_1 | ORC 203 x 9.5 | 534.7304 | 6.0835 | 40.6607 | 0.352987 | 0.00489312 | 0.000368735 | 0.352987 | Cumple |
| 106 | Columna_2 | ORC 305 x 12.7 | 1,028.44 | 28.0163 | 26.9134 | 0.340814 | 0.0155788 | 0.000102725 | 0.340814 | Cumple |
| 115 | Columna_1 | ORC 203 x 9.5 | 487.2517 | 6.3227 | 40.6607 | 0.326601 | 0.00515671 | 0.000293653 | 0.326601 | Cumple |
| 103 | Columna_1 | ORC 203 x 9.5 | 479.0602 | 6.4648 | 40.6607 | 0.321695 | 0.00525298 | 0.000218456 | 0.321695 | Cumple |
| 110 | Columna_2 | ORC 305 x 12.7 | 979.9551 | 23.2891 | 26.9134 | 0.317642 | 0.0141381 | 0.000148544 | 0.317642 | Cumple |
| 105 | Columna_1 | ORC 203 x 9.5 | 500.0245 | 0.674137 | 40.6607 | 0.296458 | 0.000478732 | 0.00387292 | 0.296458 | Cumple |
| 206 | Columna_2 | ORC 305 x 12.7 | 774.5677 | 38.9607 | 26.9134 | 0.295573 | 0.0191868 | 0.00042001 | 0.295573 | Cumple |
| 109 | Columna_1 | ORC 203 x 9.5 | 483.9200 | 0.611791 | 40.6607 | 0.286835 | 0.000486963 | 0.00386525 | 0.286835 | Cumple |
| 202 | Columna_1 | ORC 203 x 9.5 | 436.7920 | 4.2705 | 40.6607 | 0.286809 | 0.00408435 | 0.000711029 | 0.286809 | Cumple |
| 214 | Columna_1 | ORC 203 x 9.5 | 430.6887 | 4.2239 | 40.6607 | 0.284124 | 0.0039681 | 0.00100797 | 0.284124 | Cumple |
| 210 | Columna_2 | ORC 305 x 12.7 | 738.1826 | 32.6690 | 26.9134 | 0.272304 | 0.0160518 | 0.000420826 | 0.272304 | Cumple |
| 107 | Columna_2 | ORC 305 x 12.7 | 743.3444 | 21.5577 | 26.9134 | 0.265613 | 0.0109289 | 0.00683765 | 0.265613 | Cumple |
| 215 | Columna_1 | ORC 203 x 9.5 | 390.7321 | 4.5638 | 40.6607 | 0.262396 | 0.00438334 | 0.000845726 | 0.262396 | Cumple |
| 113 | Columna_1 | ORC 203 x 9.5 | 388.3648 | 5.3780 | 40.6607 | 0.261483 | 5.07624e-05 | 0.0037423 | 0.261483 | Cumple |
| 203 | Columna_1 | ORC 203 x 9.5 | 382.8190 | 4.7712 | 40.6607 | 0.255584 | 0.00465656 | 0.000343175 | 0.255584 | Cumple |
| 108 | Columna_1 | ORC 203 x 9.5 | 354.4205 | 7.3438 | 40.6607 | 0.25511 | 0.00550228 | 0.00412283 | 0.25511 | Cumple |
| 101 | Columna_1 | ORC 203 x 9.5 | 371.3291 | 5.4893 | 40.6607 | 0.252751 | 9.49393e-05 | 0.00378362 | 0.252751 | Cumple |
| 116 | Columna_1 | ORC 203 x 9.5 | 344.7969 | 5.4265 | 40.6607 | 0.236364 | 0.000137212 | 0.0038156 | 0.236364 | Cumple |
| 205 | Columna_1 | ORC 203 x 9.5 | 357.6622 | 0.379766 | 40.6607 | 0.211281 | 0.00036623 | 0.00201247 | 0.211281 | Cumple |
| 407 | Columna_2 | ORC 305 x 12.7 | 183.5119 | 49.5374 | 26.9134 | 0.18948 | 0.0120038 | 0.0169458 | 0.18948 | Cumple |
| 406 | Columna_2 | ORC 305 x 12.7 | 253.5601 | 61.9157 | 26.9134 | 0.184063 | 0.0262356 | 0.000655422 | 0.184063 | Cumple |
| 207 | Columna_2 | ORC 305 x 12.7 | 561.2771 | 29.4738 | 26.9134 | 0.174502 | 0.00867965 | 0.0119568 | 0.174502 | Cumple |
| 410 | Columna_2 | ORC 305 x 12.7 | 241.4426 | 51.3522 | 26.9134 | 0.157294 | 0.0217747 | 0.000666592 | 0.157294 | Cumple |
| 208 | Columna_1 | ORC 203 x 9.5 | 260.4934 | 10.0773 | 40.6607 | 0.151968 | 0.0100249 | 0.00254524 | 0.151968 | Cumple |
| 306 | Columna_2 | ORC 305 x 12.7 | 516.3053 | 30.5772 | 26.9134 | 0.143337 | 0.0145648 | 0.000214248 | 0.143337 | Cumple |
| 408 | Columna_1 | ORC 203 x 9.5 | 85.2606 | 15.7823 | 40.6607 | 0.141337 | 0.0136202 | 0.00230129 | 0.141337 | Cumple |
| 104 | Columna_1 | ORC 203 x 9.5 | 338.1134 | 5.5479 | 40.6607 | 0.140231 | 0.000327379 | 0.00387078 | 0.140231 | Cumple |
| 307 | Columna_2 | ORC 305 x 12.7 | 374.6219 | 23.2579 | 26.9134 | 0.128019 | 0.0080726 | 0.0090902 | 0.128019 | Cumple |
| 310 | Columna_2 | ORC 305 x 12.7 | 492.0430 | 25.3756 | 26.9134 | 0.127865 | 0.0126135 | 0.000185156 | 0.127865 | Cumple |

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
| Perfil critico | ORC 89 x 4.8 |
| Combo critico | SISMO_DOWN_1 |
| Estado control | compresion |
| P max (kN) | 77.4511 |
| PR compresion (kN) | 142.0513 |
| TR tension (kN) | 455.5609 |
| Utilizacion max | 0.545233 |

#### Calculo del Contraviento Critico

| Paso / Propiedad | Valor |
| --- | --- |
| Elemento | 367 |
| Grupo | Contraviento_2 |
| Perfil | ORC 89 x 4.8 |
| Material | A500 Gr B |
| Fy (kg/cm2) | 3,235.00 |
| A (cm2) | 15.9500 |
| b ext (mm) | 89.0000 |
| t (mm) | 4.9000 |
| lambda pared | 15.0000 |
| Clasificacion local | compacta |
| K | 1.0000 |
| L (m) | 4.4973 |
| KL/r | 132.2732 |
| Limite esbeltez | 200.0000 |
| P demanda (kN) | 77.4511 |
| PR compresion (kN) | 142.0513 |
| TR tension (kN) | 455.5609 |
| Util compresion | 0.545233 |
| Util tension | 0.170013 |
| Control | compresion |
| Estado | Cumple |

#### Resumen de Contravientos Revisados

| Elem | Grupo | Perfil | P (kN) | PR comp (kN) | TR tens (kN) | Util comp | Util tens | Control | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 367 | Contraviento_2 | ORC 89 x 4.8 | 77.4511 | 142.0513 | 455.5609 | 0.545233 | 0.170013 | compresion | Cumple |
| 355 | Contraviento_2 | ORC 89 x 4.8 | 77.0469 | 142.0513 | 455.5609 | 0.542388 | 0.169125 | compresion | Cumple |
| 356 | Contraviento_2 | ORC 89 x 4.8 | 66.1840 | 142.0513 | 455.5609 | 0.465916 | 0.14528 | compresion | Cumple |
| 368 | Contraviento_2 | ORC 89 x 4.8 | 63.9473 | 142.0513 | 455.5609 | 0.45017 | 0.14037 | compresion | Cumple |
| 358 | Contraviento_2 | ORC 89 x 4.8 | 72.4245 | 162.6363 | 455.5609 | 0.445316 | 0.158979 | compresion | Cumple |
| 370 | Contraviento_2 | ORC 89 x 4.8 | 69.1893 | 162.8874 | 455.5609 | 0.424768 | 0.151877 | compresion | Cumple |
| 468 | Contraviento_2 | ORC 89 x 4.8 | 55.9079 | 142.0513 | 455.5609 | 0.393576 | 0.122723 | compresion | Cumple |
| 156 | Contraviento_1 | ORC 114 x 5.3 | 116.6001 | 299.3877 | 598.9413 | 0.389462 | 0.194677 | compresion | Cumple |
| 268 | Contraviento_1 | ORC 114 x 5.3 | 115.8638 | 299.3877 | 598.9413 | 0.387003 | 0.193448 | compresion | Cumple |
| 256 | Contraviento_1 | ORC 114 x 5.3 | 114.9542 | 299.3877 | 598.9413 | 0.383965 | 0.191929 | compresion | Cumple |
| 168 | Contraviento_1 | ORC 114 x 5.3 | 114.7774 | 299.3877 | 598.9413 | 0.383374 | 0.191634 | compresion | Cumple |
| 456 | Contraviento_2 | ORC 89 x 4.8 | 54.2812 | 142.0513 | 455.5609 | 0.382124 | 0.119152 | compresion | Cumple |
| 155 | Contraviento_1 | ORC 114 x 5.3 | 109.5566 | 299.3877 | 598.9413 | 0.365936 | 0.182917 | compresion | Cumple |
| 167 | Contraviento_1 | ORC 114 x 5.3 | 109.5282 | 299.3877 | 598.9413 | 0.365841 | 0.18287 | compresion | Cumple |
| 255 | Contraviento_1 | ORC 114 x 5.3 | 102.4472 | 299.3877 | 598.9413 | 0.342189 | 0.171047 | compresion | Cumple |
| 357 | Contraviento_2 | ORC 89 x 4.8 | 55.2613 | 162.6363 | 455.5609 | 0.339785 | 0.121304 | compresion | Cumple |
| 457 | Contraviento_2 | ORC 89 x 4.8 | 54.9983 | 162.6363 | 455.5609 | 0.338167 | 0.120727 | compresion | Cumple |
| 267 | Contraviento_1 | ORC 114 x 5.3 | 99.9788 | 299.3877 | 598.9413 | 0.333944 | 0.166926 | compresion | Cumple |
| 257 | Contraviento_1 | ORC 114 x 5.3 | 107.0403 | 326.8518 | 598.9413 | 0.327489 | 0.178716 | compresion | Cumple |
| 369 | Contraviento_2 | ORC 89 x 4.8 | 52.8156 | 162.3856 | 455.5609 | 0.325248 | 0.115935 | compresion | Cumple |
| 269 | Contraviento_1 | ORC 114 x 5.3 | 103.9564 | 326.5463 | 598.9413 | 0.318351 | 0.173567 | compresion | Cumple |
| 169 | Contraviento_1 | ORC 114 x 5.3 | 102.7804 | 326.5463 | 598.9413 | 0.31475 | 0.171603 | compresion | Cumple |
| 469 | Contraviento_2 | ORC 89 x 4.8 | 51.0561 | 162.3856 | 455.5609 | 0.314412 | 0.112073 | compresion | Cumple |
| 157 | Contraviento_1 | ORC 114 x 5.3 | 102.5723 | 326.8518 | 598.9413 | 0.313819 | 0.171256 | compresion | Cumple |
| 158 | Contraviento_1 | ORC 114 x 5.3 | 101.8352 | 326.8518 | 598.9413 | 0.311564 | 0.170025 | compresion | Cumple |
| 170 | Contraviento_1 | ORC 114 x 5.3 | 100.8102 | 327.1570 | 598.9413 | 0.30814 | 0.168314 | compresion | Cumple |
| 455 | Contraviento_2 | ORC 89 x 4.8 | 38.5296 | 142.0513 | 455.5609 | 0.271237 | 0.0845762 | compresion | Cumple |
| 467 | Contraviento_2 | ORC 89 x 4.8 | 37.7609 | 142.0513 | 455.5609 | 0.265826 | 0.0828888 | compresion | Cumple |
| 270 | Contraviento_1 | ORC 114 x 5.3 | 83.7739 | 327.1570 | 598.9413 | 0.256066 | 0.13987 | compresion | Cumple |
| 258 | Contraviento_1 | ORC 114 x 5.3 | 81.1118 | 326.8518 | 598.9413 | 0.248161 | 0.135425 | compresion | Cumple |

#### Alertas

_Sin alertas generales._

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 330 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 71,055.31 | 103,963.68 | 0.683463 | Cumple |
| 130 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 70,731.87 | 103,963.68 | 0.680352 | Cumple |
| 230 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 70,719.92 | 103,963.68 | 0.680237 | Cumple |
| 430 | Viga_2 | IR 203 x 31.3 | GRAV_DISEÑO | 67,824.77 | 103,963.68 | 0.652389 | Cumple |
| 137 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 59,319.57 | 103,963.68 | 0.57058 | Cumple |
| 337 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 59,178.66 | 103,963.68 | 0.569224 | Cumple |
| 237 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 59,077.76 | 103,963.68 | 0.568254 | Cumple |
| 437 | Viga_2 | IR 203 x 31.3 | GRAV_DISEÑO | 56,466.30 | 103,963.68 | 0.543135 | Cumple |
| 431 | Viga_2 | IR 203 x 31.3 | GRAV_DISEÑO | 44,852.72 | 103,963.68 | 0.431427 | Cumple |
| 131 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 43,050.25 | 103,963.68 | 0.414089 | Cumple |
| 231 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 42,959.43 | 103,963.68 | 0.413216 | Cumple |
| 331 | Viga_1 | IR 203 x 31.3 | GRAV_DISEÑO | 42,631.68 | 103,963.68 | 0.410063 | Cumple |

### 10.2 Contravientos Criticos

| Elem | Grupo | Perfil | Combo | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 367 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 77,451.09 | 0 | 0.545772 | Cumple |
| 355 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 77,046.91 | 0 | 0.542924 | Cumple |
| 356 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 66,183.97 | 0 | 0.466376 | Cumple |
| 368 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 63,947.25 | 0 | 0.450615 | Cumple |
| 358 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 72,424.52 | 0 | 0.445756 | Cumple |
| 370 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 69,189.32 | 0 | 0.425188 | Cumple |
| 468 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 55,907.91 | 0 | 0.393964 | Cumple |
| 156 | Contraviento_1 | ORC 114 x 5.3 | SISMO_DOWN_1 | 116,600.12 | 0 | 0.389026 | Cumple |
| 268 | Contraviento_1 | ORC 114 x 5.3 | SISMO_DOWN_1 | 115,863.79 | 0 | 0.386569 | Cumple |
| 256 | Contraviento_1 | ORC 114 x 5.3 | SISMO_DOWN_1 | 114,954.25 | 0 | 0.383535 | Cumple |
| 168 | Contraviento_1 | ORC 114 x 5.3 | SISMO_DOWN_1 | 114,777.38 | 0 | 0.382945 | Cumple |
| 456 | Contraviento_2 | ORC 89 x 4.8 | SISMO_DOWN_1 | 54,281.21 | 0 | 0.382502 | Cumple |

## 11. Dictamen Automatico

**Estado global:** REVISAR

El modelo analizado presenta al menos un criterio en estado Revisar; no debe emitirse como diseno final sin ajuste o revision tecnica adicional.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
