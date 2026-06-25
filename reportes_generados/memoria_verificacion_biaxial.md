# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-06-04 07:41:01

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 100 |
| Elementos | 184 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 160,275.72 |
| Masa sismica (kg) | 751,822.13 |

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
| Masa propia elementos (kg) | 160,275.72 |
| Masa sismica total (kg) | 751,822.13 |
| Peso sismico total (N) | 7,375,375.09 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 16,724.48 | 16,724.48 |
| 3.2000 | 127,052.69 | 27,323.16 | 40,068.93 | 194,444.78 |
| 6.4000 | 127,052.69 | 27,323.16 | 40,068.93 | 194,444.78 |
| 9.6000 | 127,052.69 | 27,323.16 | 40,068.93 | 194,444.78 |
| 12.8000 | 109,292.64 | 19,126.21 | 23,344.45 | 151,763.31 |

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
| 1 | 0.753598 |
| 2 | 0.692795 |
| 3 | 0.62173 |
| 4 | 0.186946 |
| 5 | 0.17799 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 3.9301e-06 |
| 2 |  |  | 0.760414 |
| 3 |  |  | 0.762352 |
| 4 |  |  | 0.762362 |
| 5 |  |  | 0.914581 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.751845 |
| 2 |  |  | 0.751849 |
| 3 |  |  | 0.751851 |
| 4 |  |  | 0.910412 |
| 5 |  |  | 0.91044 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.439715 | 1.0000 | Cumple |
| Deriva servicio | 0.00184904 | 0.005 | Cumple |
| Deriva colapso | 0.00821795 | 0.01 | Cumple |
| Desp. azotea | 0.0194185 | 0.1 | Cumple |
| KL/r | 20.7423 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.00088027 | 0.00391231 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.00177173 | 0.00787434 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.00184904 | 0.00821795 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.00156725 | 0.00696557 |

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
| Elemento | 106 |
| Grupo | Columna_1 |
| Perfil | MIXTA-ORC 457 x 15.9 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.0453982 |
| Ix (m4) | 0.0010805 |
| Iy (m4) | 0.0010805 |
| r_min (m) | 0.154274 |
| KL/r | 20.7423 |
| Pu (kN) | 54.8622 |
| Mu (kN-m) | 189.2529 |
| My local (kN-m) | 189.2519 |
| Mz local (kN-m) | 0.615341 |
| phi Pn (kN) | 8,729.70 |
| phi Mny (kN-m) | 1,553.18 |
| phi Mnz (kN-m) | 1,553.18 |
| Extremo critico | envolvente |
| Tipo revision | MIXTA-OR biaxial simetrica: misma capacidad en ambos ejes locales |

Sustitucion numerica:

- `Pu/phiPn = 54.862 / 8729.696 = 0.0063`
- `My/phiMny = 189.252 / 1553.175 = 0.1218`
- `Mz/phiMnz = 0.615 / 1553.175 = 0.0004`
- `Suma flexion biaxial = 0.1222`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`
- `D/C = 0.1254`
- Estado: **Cumple**

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | My (N-m) | Mz (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 106 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 54,862.19 | 189,251.86 | 615.3415 | 0.125387 | Cumple |
| 110 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 52,214.83 | 180,692.12 | 615.3415 | 0.119724 | Cumple |
| 103 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 47,670.31 | 174,649.96 | 615.3415 | 0.115574 | Cumple |
| 107 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 51,012.53 | 174,094.80 | 615.3415 | 0.115408 | Cumple |
| 102 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 46,779.48 | 173,677.72 | 615.3415 | 0.114897 | Cumple |
| 111 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 48,948.21 | 167,822.14 | 615.3415 | 0.111251 | Cumple |
| 104 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 40,786.36 | 167,263.19 | 615.3415 | 0.110423 | Cumple |
| 101 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 39,300.06 | 165,588.35 | 615.3415 | 0.10926 | Cumple |
| 108 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 39,602.61 | 161,878.50 | 615.3415 | 0.106889 | Cumple |
| 105 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 38,279.00 | 160,382.29 | 615.3415 | 0.10585 | Cumple |
| 115 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 43,551.64 | 157,940.53 | 615.3415 | 0.104579 | Cumple |
| 114 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 42,753.43 | 157,067.25 | 615.3415 | 0.103971 | Cumple |
| 112 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 38,467.23 | 156,602.53 | 615.3415 | 0.103427 | Cumple |
| 109 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 37,172.02 | 155,143.14 | 615.3415 | 0.102413 | Cumple |
| 116 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 37,374.91 | 151,315.52 | 615.3415 | 0.0999602 | Cumple |
| 113 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 36,038.54 | 149,827.68 | 615.3415 | 0.0989257 | Cumple |
| 206 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 62,463.96 | 123,790.82 | 1,332.52 | 0.0841374 | Cumple |
| 210 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 58,513.67 | 116,431.55 | 1,332.52 | 0.0791729 | Cumple |
| 207 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 56,137.98 | 115,325.77 | 1,332.52 | 0.0783249 | Cumple |
| 211 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 53,214.54 | 109,296.52 | 1,332.52 | 0.0742756 | Cumple |

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 226 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 105,893.63 | 240,823.34 | 0.439715 | Cumple |
| 326 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 101,110.42 | 240,823.34 | 0.419853 | Cumple |
| 126 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 97,296.37 | 240,823.34 | 0.404016 | Cumple |
| 232 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 95,271.03 | 240,823.34 | 0.395605 | Cumple |
| 332 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 90,559.58 | 240,823.34 | 0.376042 | Cumple |
| 132 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 86,891.73 | 240,823.34 | 0.360811 | Cumple |
| 227 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 83,969.93 | 240,823.34 | 0.348679 | Cumple |
| 426 | Viga_2 | IR 356 x 44.6 | SISMO_DOWN_1 | 80,255.93 | 240,823.34 | 0.333256 | Cumple |
| 327 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 79,592.55 | 240,823.34 | 0.330502 | Cumple |
| 233 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 77,140.09 | 240,823.34 | 0.320318 | Cumple |
| 127 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 74,276.16 | 240,823.34 | 0.308426 | Cumple |
| 333 | Viga_1 | IR 356 x 44.6 | SISMO_DOWN_1 | 72,882.99 | 240,823.34 | 0.302641 | Cumple |

### 10.2 Contravientos Criticos

_Sin datos disponibles._

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
