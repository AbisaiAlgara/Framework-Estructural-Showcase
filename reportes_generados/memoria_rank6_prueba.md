# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-05-27 11:09:32

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 132 |
| Elementos | 280 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 40,083.70 |
| Masa sismica (kg) | 608,405.42 |

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
| Masa propia elementos (kg) | 40,083.70 |
| Masa sismica total (kg) | 608,405.42 |
| Peso sismico total (N) | 5,968,457.21 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 2,136.49 | 2,136.49 |
| 3.2000 | 122,954.22 | 19,126.21 | 10,462.50 | 152,542.94 |
| 6.4000 | 122,954.22 | 19,126.21 | 10,462.50 | 152,542.94 |
| 9.6000 | 122,954.22 | 19,126.21 | 10,462.50 | 152,542.94 |
| 12.8000 | 122,954.22 | 19,126.21 | 6,559.70 | 148,640.13 |

## 5. Analisis Modal

El numero de modos se selecciona buscando alcanzar la participacion modal minima configurada.

| Parametro | Valor |
| --- | --- |
| Objetivo participacion | 0.9 |
| Modos usados | 10 |
| Maximo modos |  |

### 5.1 Periodos

| Modo | Periodo (s) |
| --- | --- |
| 1 | 0.452566 |
| 2 | 0.449029 |
| 3 | 0.289099 |
| 4 | 0.288946 |
| 5 | 0.21901 |
| 6 | 0.194571 |
| 7 | 0.191708 |
| 8 | 0.191295 |
| 9 | 0.189755 |
| 10 | 0.183112 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.825614 |
| 2 |  |  | 0.826914 |
| 3 |  |  | 0.826915 |
| 4 |  |  | 0.826996 |
| 5 |  |  | 0.827001 |
| 6 |  |  | 0.827003 |
| 7 |  |  | 0.827035 |
| 8 |  |  | 0.827036 |
| 9 |  |  | 0.827043 |
| 10 |  |  | 0.827071 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.00127556 |
| 2 |  |  | 0.827749 |
| 3 |  |  | 0.827859 |
| 4 |  |  | 0.829554 |
| 5 |  |  | 0.82956 |
| 6 |  |  | 0.829562 |
| 7 |  |  | 0.829692 |
| 8 |  |  | 0.830621 |
| 9 |  |  | 0.830642 |
| 10 |  |  | 0.830721 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.991197 | 1.0000 | Cumple |
| Deriva servicio | 0.000931503 | 0.004 | Cumple |
| Deriva colapso | 0.00414001 | 0.015 | Cumple |
| Desp. azotea | 0.00952844 | 0.1 | Cumple |
| KL/r | 94.0467 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000654149 | 0.00290733 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.000931503 | 0.00414001 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.000776933 | 0.00345304 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.000615053 | 0.00273357 |

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
| Perfil | IR 406 x 59.5 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.00762 |
| Ix (m4) | 0.00021561 |
| Iy (m4) | 1.203e-05 |
| r_min (m) | 0.0397334 |
| KL/r | 80.5369 |
| Pu (kN) | 12.2682 |
| Mu (kN-m) | 30.7387 |
| phi Pn (kN) | 1,209.93 |
| phi Mn (kN-m) | 267.3783 |

Sustitucion numerica:

- `Pu/phiPn = 12.268 / 1209.932 = 0.0101`
- `Mu/phiMn = 30.739 / 267.378 = 0.1150`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + Mu/phiMn`
- `D/C = 0.1200`
- Estado: **Cumple**

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 103 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 12,268.16 | 30,738.71 | 0.120033 | Cumple |
| 102 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 11,827.41 | 30,663.87 | 0.119571 | Cumple |
| 115 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 12,050.90 | 30,366.23 | 0.11855 | Cumple |
| 105 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,973.19 | 30,295.57 | 0.117841 | Cumple |
| 114 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 11,463.11 | 29,658.35 | 0.11566 | Cumple |
| 109 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,677.67 | 29,679.72 | 0.115415 | Cumple |
| 108 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,460.78 | 29,312.71 | 0.113953 | Cumple |
| 112 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,222.66 | 28,716.35 | 0.111624 | Cumple |
| 104 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,170.68 | 28,607.86 | 0.111197 | Cumple |
| 101 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,150.61 | 28,377.02 | 0.110325 | Cumple |
| 116 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 9,959.17 | 27,907.76 | 0.108491 | Cumple |
| 113 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 10,010.00 | 27,845.22 | 0.108278 | Cumple |
| 406 | Columna_2 | IR 610 x 81.8 | GRAV_DISEÑO | 94.0467 | 17,937.15 | 33,285.49 | 0.0739698 | Cumple |
| 410 | Columna_2 | IR 610 x 81.8 | GRAV_DISEÑO | 94.0467 | 14,983.91 | 27,850.10 | 0.0618826 | Cumple |
| 203 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 6,123.50 | 10,970.28 | 0.0435596 | Cumple |
| 206 | Columna_2 | IR 610 x 81.8 | GRAV_DISEÑO | 94.0467 | 11,419.21 | 18,523.13 | 0.0416535 | Cumple |
| 215 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 5,730.04 | 10,183.60 | 0.0404548 | Cumple |
| 303 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 4,713.81 | 10,189.26 | 0.040056 | Cumple |
| 214 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 5,267.11 | 10,107.43 | 0.0399786 | Cumple |
| 315 | Columna_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 80.5369 | 4,599.01 | 10,002.95 | 0.0393117 | Cumple |

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 430 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 54,466.21 | 54,949.93 | 0.991197 | Cumple |
| 431 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 54,371.51 | 54,949.93 | 0.989474 | Cumple |
| 437 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 46,157.86 | 54,949.93 | 0.839998 | Cumple |
| 438 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 46,129.68 | 54,949.93 | 0.839486 | Cumple |
| 330 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 57,326.88 | 99,624.67 | 0.575429 | Cumple |
| 130 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 55,584.47 | 99,624.67 | 0.557939 | Cumple |
| 131 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 55,493.98 | 99,624.67 | 0.557031 | Cumple |
| 230 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 55,102.32 | 99,624.67 | 0.553099 | Cumple |
| 231 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 54,562.01 | 99,624.67 | 0.547676 | Cumple |
| 331 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 52,942.74 | 99,624.67 | 0.531422 | Cumple |
| 337 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 47,713.84 | 99,624.67 | 0.478936 | Cumple |
| 137 | Viga_1 | IR 203 x 41.7 | GRAV_DISEÑO | 47,095.39 | 99,624.67 | 0.472728 | Cumple |

### 10.2 Contravientos Criticos

| Elem | Grupo | Perfil | Combo | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 268 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 103,263.46 | 0 | 0.728421 | Cumple |
| 256 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 103,043.26 | 0 | 0.726868 | Cumple |
| 156 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 95,426.55 | 0 | 0.673139 | Cumple |
| 168 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 93,604.04 | 0 | 0.660283 | Cumple |
| 155 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 92,248.39 | 0 | 0.650721 | Cumple |
| 167 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 91,921.09 | 0 | 0.648412 | Cumple |
| 255 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 91,233.47 | 0 | 0.643561 | Cumple |
| 267 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 88,888.28 | 0 | 0.627018 | Cumple |
| 257 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 93,968.90 | 0 | 0.589879 | Cumple |
| 269 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 91,136.32 | 0 | 0.572809 | Cumple |
| 157 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 85,696.34 | 0 | 0.537949 | Cumple |
| 169 | Contraviento_1 | ORC 89 x 4.8 | SISMO_DOWN_1 | 85,529.98 | 0 | 0.537572 | Cumple |

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
