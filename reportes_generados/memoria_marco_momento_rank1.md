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
| Peso de acero (kg) | 45,231.55 |
| Masa sismica (kg) | 613,553.28 |

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
| Masa propia elementos (kg) | 45,231.55 |
| Masa sismica total (kg) | 613,553.28 |
| Peso sismico total (N) | 6,018,957.69 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 2,590.72 | 2,590.72 |
| 3.2000 | 122,954.22 | 19,126.21 | 11,801.42 | 153,881.85 |
| 6.4000 | 122,954.22 | 19,126.21 | 11,801.42 | 153,881.85 |
| 9.6000 | 122,954.22 | 19,126.21 | 11,801.42 | 153,881.85 |
| 12.8000 | 122,954.22 | 19,126.21 | 7,236.58 | 149,317.01 |

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
| 1 | 1.2240 |
| 2 | 0.996385 |
| 3 | 0.932822 |
| 4 | 0.393267 |
| 5 | 0.304896 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 2.88341e-07 |
| 2 |  |  | 0.779108 |
| 3 |  |  | 0.787203 |
| 4 |  |  | 0.787203 |
| 5 |  |  | 0.918759 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.806318 |
| 2 |  |  | 0.806332 |
| 3 |  |  | 0.807375 |
| 4 |  |  | 0.927933 |
| 5 |  |  | 0.927944 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.796818 | 1.0000 | Cumple |
| Deriva servicio | 0.00299897 | 0.004 | Cumple |
| Deriva colapso | 0.0133288 | 0.015 | Cumple |
| Desp. azotea | 0.0332056 | 0.1 | Cumple |
| KL/r | 51.2354 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.00171631 | 0.00762806 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.00299897 | 0.0133288 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.00290851 | 0.0129267 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.00275296 | 0.0122354 |

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
| Perfil | IR 356 x 101.2 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.01291 |
| Ix (m4) | 0.00030052 |
| Iy (m4) | 5.036e-05 |
| r_min (m) | 0.0624568 |
| KL/r | 51.2354 |
| Pu (kN) | 45.0307 |
| Mu (kN-m) | 115.7926 |
| phi Pn (kN) | 2,511.71 |
| phi Mn (kN-m) | 421.0594 |

Sustitucion numerica:

- `Pu/phiPn = 45.031 / 2511.705 = 0.0179`
- `Mu/phiMn = 115.793 / 421.059 = 0.2750`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + Mu/phiMn`
- `D/C = 0.2840`
- Estado: **Cumple**

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 103 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 45,030.73 | 115,792.61 | 0.283967 | Cumple |
| 102 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 43,748.13 | 114,397.06 | 0.280397 | Cumple |
| 104 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 35,546.30 | 105,721.41 | 0.25816 | Cumple |
| 101 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 33,665.53 | 103,540.81 | 0.252607 | Cumple |
| 115 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 37,667.08 | 97,019.05 | 0.237915 | Cumple |
| 105 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 31,545.00 | 97,495.16 | 0.237827 | Cumple |
| 108 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 31,428.93 | 97,498.62 | 0.237812 | Cumple |
| 114 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 36,714.51 | 95,923.13 | 0.235122 | Cumple |
| 112 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 29,822.48 | 92,103.20 | 0.224678 | Cumple |
| 109 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 29,620.27 | 91,705.32 | 0.223693 | Cumple |
| 116 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 29,815.33 | 88,955.46 | 0.217201 | Cumple |
| 113 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 29,023.64 | 87,748.71 | 0.214177 | Cumple |
| 203 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 45,941.05 | 79,182.26 | 0.1972 | Cumple |
| 202 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 43,266.87 | 74,920.07 | 0.186545 | Cumple |
| 303 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 37,879.37 | 67,680.62 | 0.168279 | Cumple |
| 215 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 38,164.65 | 65,639.66 | 0.163489 | Cumple |
| 302 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 34,948.49 | 63,181.37 | 0.15701 | Cumple |
| 214 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 36,335.21 | 62,556.84 | 0.155803 | Cumple |
| 406 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 32,495.91 | 59,192.71 | 0.147049 | Cumple |
| 315 | Columna_1 | IR 356 x 101.2 | SISMO_DOWN_1 | 51.2354 | 31,411.04 | 55,661.27 | 0.138446 | Cumple |

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 426 | Viga_2 | IR 203 x 31.3 | GRAV_DISEÑO | 59,626.03 | 74,830.19 | 0.796818 | Cumple |
| 427 | Viga_2 | IR 203 x 31.3 | GRAV_DISEÑO | 50,513.49 | 74,830.19 | 0.675042 | Cumple |
| 432 | Viga_2 | IR 203 x 31.3 | GRAV_DISEÑO | 49,883.89 | 74,830.19 | 0.666628 | Cumple |
| 433 | Viga_2 | IR 203 x 31.3 | SISMO_DOWN_1 | 43,276.11 | 74,830.19 | 0.578324 | Cumple |
| 226 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 72,875.62 | 157,925.21 | 0.461457 | Cumple |
| 326 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 71,322.92 | 157,925.21 | 0.451625 | Cumple |
| 227 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 68,341.08 | 157,925.21 | 0.432743 | Cumple |
| 126 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 67,887.13 | 157,925.21 | 0.429869 | Cumple |
| 127 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 63,829.59 | 157,925.21 | 0.404176 | Cumple |
| 232 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 63,771.52 | 157,925.21 | 0.403808 | Cumple |
| 327 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 63,719.49 | 157,925.21 | 0.403479 | Cumple |
| 221 | Viga_1 | IR 305 x 44.6 | SISMO_DOWN_1 | 63,628.21 | 157,925.21 | 0.402901 | Cumple |

### 10.2 Contravientos Criticos

_Sin datos disponibles._

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
