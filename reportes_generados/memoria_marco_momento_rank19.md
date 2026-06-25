# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-05-28 22:55:58

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 100 |
| Elementos | 184 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 75,779.28 |
| Masa sismica (kg) | 644,101.01 |

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
| Masa propia elementos (kg) | 75,779.28 |
| Masa sismica total (kg) | 644,101.01 |
| Peso sismico total (N) | 6,318,630.87 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 4,229.12 | 4,229.12 |
| 3.2000 | 122,954.22 | 19,126.21 | 20,599.81 | 162,680.25 |
| 6.4000 | 122,954.22 | 19,126.21 | 20,599.81 | 162,680.25 |
| 9.6000 | 122,954.22 | 19,126.21 | 20,599.81 | 162,680.25 |
| 12.8000 | 122,954.22 | 19,126.21 | 9,750.72 | 151,831.15 |

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
| 1 | 0.711426 |
| 2 | 0.559314 |
| 3 | 0.526071 |
| 4 | 0.248514 |
| 5 | 0.236776 |
| 6 | 0.179692 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 9.68121e-08 |
| 2 |  |  | 0.753468 |
| 3 |  |  | 0.762219 |
| 4 |  |  | 0.762219 |
| 5 |  |  | 0.762632 |
| 6 |  |  | 0.902479 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.781774 |
| 2 |  |  | 0.781787 |
| 3 |  |  | 0.782784 |
| 4 |  |  | 0.918658 |
| 5 |  |  | 0.919109 |
| 6 |  |  | 0.91914 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.975843 | 1.0000 | Cumple |
| Deriva servicio | 0.00141439 | 0.004 | Cumple |
| Deriva colapso | 0.00628618 | 0.015 | Cumple |
| Desp. azotea | 0.0143411 | 0.1 | Cumple |
| KL/r | 43.5255 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000657524 | 0.00292233 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.00117952 | 0.00524233 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.00123015 | 0.00546732 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.00141439 | 0.00628618 |

## 9. Diseno de Columnas

### 9.1 Metodologia

Para cada columna se obtiene la envolvente de fuerzas `Pu` y `Mu` por combinacion. La resistencia axial se evalua con la capacidad `phi Pn` de la seccion considerando esbeltez, y la resistencia a flexion con `phi Mn`.

El radio de giro minimo se calcula como:

`r_min = sqrt(min(Ix, Iy) / A)`

La esbeltez se calcula como:

`KL/r = K L / r_min`

La interaccion flexocompresion se evalua con:

`Si Pu/phiPn >= 0.20: D/C = Pu/phiPn + 8/9 (Mu/phiMn)`

`Si Pu/phiPn < 0.20: D/C = Pu/(2 phiPn) + Mu/phiMn`

El criterio de aceptacion es `D/C <= 1.0`.

### 9.2 Columna Critica Desarrollada

| Dato | Valor |
| --- | --- |
| Elemento | 103 |
| Grupo | Columna_1 |
| Perfil | IR 533 x 165.2 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.0211 |
| Ix (m4) | 0.00111134 |
| Iy (m4) | 0.00011405 |
| r_min (m) | 0.0735202 |
| KL/r | 43.5255 |
| Pu (kN) | 63.4165 |
| Mu (kN-m) | 163.7696 |
| phi Pn (kN) | 4,266.00 |
| phi Mn (kN-m) | 1,021.26 |

Sustitucion numerica:

- `Pu/phiPn = 63.417 / 4266.001 = 0.0149`
- `Mu/phiMn = 163.770 / 1021.265 = 0.1604`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + Mu/phiMn`
- `D/C = 0.1678`
- Estado: **Cumple**

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 103 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 63,416.54 | 163,769.56 | 0.167792 | Cumple |
| 102 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 61,428.09 | 161,630.08 | 0.165464 | Cumple |
| 104 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 49,830.65 | 149,270.31 | 0.152003 | Cumple |
| 101 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 47,258.59 | 146,768.38 | 0.149251 | Cumple |
| 115 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 52,253.40 | 136,535.27 | 0.139817 | Cumple |
| 108 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 43,053.54 | 136,414.92 | 0.138621 | Cumple |
| 114 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 51,149.39 | 135,307.74 | 0.138485 | Cumple |
| 105 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 42,495.50 | 135,750.07 | 0.137904 | Cumple |
| 112 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 40,514.78 | 128,327.03 | 0.130404 | Cumple |
| 109 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 39,631.32 | 127,218.57 | 0.129215 | Cumple |
| 116 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 41,089.75 | 125,164.68 | 0.127374 | Cumple |
| 113 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 40,111.27 | 123,474.94 | 0.125605 | Cumple |
| 203 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 65,419.29 | 115,777.24 | 0.121034 | Cumple |
| 202 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 61,279.49 | 109,225.58 | 0.114134 | Cumple |
| 303 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 57,862.72 | 97,253.57 | 0.10201 | Cumple |
| 215 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 54,291.86 | 96,555.36 | 0.100908 | Cumple |
| 214 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 52,269.63 | 93,065.09 | 0.0972536 | Cumple |
| 403 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 34,550.83 | 94,675.33 | 0.0967536 | Cumple |
| 302 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 52,313.17 | 88,681.65 | 0.0929665 | Cumple |
| 402 | Columna_1 | IR 533 x 165.2 | SISMO_DOWN_1 | 43.5255 | 32,314.67 | 88,683.20 | 0.0906241 | Cumple |

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 426 | Viga_2 | IR 152 x 37.2 | GRAV_DISEÑO | 67,573.06 | 69,245.85 | 0.975843 | Cumple |
| 432 | Viga_2 | IR 152 x 37.2 | GRAV_DISEÑO | 56,775.46 | 69,245.85 | 0.819911 | Cumple |
| 427 | Viga_2 | IR 152 x 37.2 | GRAV_DISEÑO | 45,462.92 | 69,245.85 | 0.656544 | Cumple |
| 433 | Viga_2 | IR 152 x 37.2 | GRAV_DISEÑO | 38,454.09 | 69,245.85 | 0.555327 | Cumple |
| 321 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 98,912.76 | 410,114.11 | 0.241183 | Cumple |
| 221 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 96,109.57 | 410,114.11 | 0.234348 | Cumple |
| 319 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 92,501.02 | 410,114.11 | 0.225549 | Cumple |
| 219 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 92,394.75 | 410,114.11 | 0.22529 | Cumple |
| 121 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 82,445.86 | 410,114.11 | 0.201032 | Cumple |
| 317 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 81,648.27 | 410,114.11 | 0.199087 | Cumple |
| 340 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 80,553.75 | 410,114.11 | 0.196418 | Cumple |
| 240 | Viga_1 | IR 457 x 81.8 | SISMO_DOWN_1 | 80,356.04 | 410,114.11 | 0.195936 | Cumple |

### 10.2 Contravientos Criticos

_Sin datos disponibles._

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
