# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-06-05 05:42:42

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 132 |
| Elementos | 280 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 166,111.30 |
| Masa sismica (kg) | 734,433.03 |

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
| Masa propia elementos (kg) | 166,111.30 |
| Masa sismica total (kg) | 734,433.03 |
| Peso sismico total (N) | 7,204,787.99 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 17,453.92 | 17,453.92 |
| 3.2000 | 122,954.22 | 19,126.21 | 41,527.82 | 183,608.26 |
| 6.4000 | 122,954.22 | 19,126.21 | 41,527.82 | 183,608.26 |
| 9.6000 | 122,954.22 | 19,126.21 | 41,527.82 | 183,608.26 |
| 12.8000 | 122,954.22 | 19,126.21 | 24,073.90 | 166,154.33 |

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
| 1 | 0.326325 |
| 2 | 0.326174 |
| 3 | 0.226586 |
| 4 | 0.105213 |
| 5 | 0.105016 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.352445 |
| 2 |  |  | 0.827002 |
| 3 |  |  | 0.827103 |
| 4 |  |  | 0.864204 |
| 5 |  |  | 0.922774 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.473015 |
| 2 |  |  | 0.824957 |
| 3 |  |  | 0.826527 |
| 4 |  |  | 0.890908 |
| 5 |  |  | 0.937601 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.299212 | 1.0000 | Cumple |
| Deriva servicio | 0.000427637 | 0.005 | Cumple |
| Deriva colapso | 0.00190061 | 0.01 | Cumple |
| Desp. azotea | 0.0042088 | 0.1 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000284006 | 0.00126225 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.000427637 | 0.00190061 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.000352449 | 0.00156644 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.000251158 | 0.00111626 |

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
| Grupo | Columna_2 |
| Perfil | MIXTA-ORC 457 x 15.9 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.0453982 |
| Ix (m4) | 0.0010805 |
| Iy (m4) | 0.0010805 |
| r_min (m) | 0.154274 |
| KL/r | 20.7423 |
| Pu (kN) | 27.4489 |
| Mu (kN-m) | 71.8211 |
| My local (kN-m) | 71.8211 |
| Mz local (kN-m) | 0.0677993 |
| phi Pn (kN) | 8,729.70 |
| phi Mny (kN-m) | 1,553.18 |
| phi Mnz (kN-m) | 1,553.18 |
| Extremo critico | envolvente |
| Tipo revision | MIXTA-OR biaxial simetrica: misma capacidad en ambos ejes locales |

Sustitucion numerica:

- `Pu/phiPn = 27.449 / 8729.696 = 0.0031`
- `My/phiMny = 71.821 / 1553.175 = 0.0462`
- `Mz/phiMnz = 0.068 / 1553.175 = 0.0000`
- `Suma flexion biaxial = 0.0463`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`
- `D/C = 0.0479`
- Estado: **Cumple**

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | My (N-m) | Mz (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 106 | Columna_2 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 27,448.87 | 71,821.05 | 67.7993 | 0.0478572 | Cumple |
| 110 | Columna_2 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 25,766.87 | 68,087.08 | 67.7993 | 0.0453568 | Cumple |
| 406 | Columna_2 | MIXTA-ORC 457 x 15.9 | GRAV_DISEÑO | 20.7423 | 33,823.82 | 65,246.87 | 122.6533 | 0.0440249 | Cumple |
| 107 | Columna_2 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 22,081.63 | 58,437.94 | 67.7993 | 0.0389332 | Cumple |
| 111 | Columna_2 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 21,412.81 | 57,204.67 | 67.7993 | 0.0381009 | Cumple |
| 410 | Columna_2 | MIXTA-ORC 457 x 15.9 | GRAV_DISEÑO | 20.7423 | 28,046.03 | 54,041.84 | 122.6533 | 0.0364798 | Cumple |
| 103 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 18,122.24 | 51,285.09 | 67.7993 | 0.0341011 | Cumple |
| 115 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 18,068.54 | 50,801.44 | 67.7993 | 0.0337867 | Cumple |
| 102 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 17,338.36 | 50,501.45 | 67.7993 | 0.0335517 | Cumple |
| 114 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 17,412.88 | 50,212.58 | 67.7993 | 0.03337 | Cumple |
| 104 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 16,046.09 | 49,118.88 | 67.7993 | 0.0325875 | Cumple |
| 108 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 16,189.43 | 48,915.60 | 67.7993 | 0.0324648 | Cumple |
| 101 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 15,849.65 | 48,855.79 | 67.7993 | 0.0324069 | Cumple |
| 112 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 16,170.56 | 48,755.00 | 67.7993 | 0.0323604 | Cumple |
| 116 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 16,023.68 | 48,670.66 | 67.7993 | 0.0322977 | Cumple |
| 113 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 15,834.28 | 48,414.64 | 67.7993 | 0.032122 | Cumple |
| 105 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 15,555.33 | 48,278.49 | 67.7993 | 0.0320183 | Cumple |
| 109 | Columna_1 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 15,611.33 | 48,239.85 | 67.7993 | 0.0319967 | Cumple |
| 206 | Columna_2 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 26,435.80 | 44,790.44 | 107.0169 | 0.030421 | Cumple |
| 210 | Columna_2 | MIXTA-ORC 457 x 15.9 | SISMO_DOWN_1 | 20.7423 | 23,323.23 | 39,732.84 | 107.0169 | 0.0269864 | Cumple |

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 130 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 72,057.29 | 240,823.34 | 0.299212 | Cumple |
| 330 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 71,982.72 | 240,823.34 | 0.298903 | Cumple |
| 230 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 71,846.40 | 240,823.34 | 0.298337 | Cumple |
| 430 | Viga_2 | IR 356 x 44.6 | GRAV_DISEÑO | 69,557.35 | 240,823.34 | 0.288831 | Cumple |
| 137 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 60,418.49 | 240,823.34 | 0.250883 | Cumple |
| 237 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 59,939.38 | 240,823.34 | 0.248894 | Cumple |
| 337 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 59,846.25 | 240,823.34 | 0.248507 | Cumple |
| 437 | Viga_2 | IR 356 x 44.6 | GRAV_DISEÑO | 57,759.04 | 240,823.34 | 0.23984 | Cumple |
| 431 | Viga_2 | IR 356 x 44.6 | GRAV_DISEÑO | 44,683.14 | 240,823.34 | 0.185543 | Cumple |
| 231 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 43,176.23 | 240,823.34 | 0.179286 | Cumple |
| 131 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 43,138.96 | 240,823.34 | 0.179131 | Cumple |
| 331 | Viga_1 | IR 356 x 44.6 | GRAV_DISEÑO | 42,981.64 | 240,823.34 | 0.178478 | Cumple |

### 10.2 Contravientos Criticos

| Elem | Grupo | Perfil | Combo | Pu (N) | Mu (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 268 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 62,400.08 | 0 | 0.0896111 | Cumple |
| 255 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 60,817.99 | 0 | 0.0873391 | Cumple |
| 256 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 60,763.06 | 0 | 0.0872602 | Cumple |
| 267 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 59,850.96 | 0 | 0.0859504 | Cumple |
| 257 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 59,643.03 | 0 | 0.0783242 | Cumple |
| 269 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 58,522.69 | 0 | 0.0769263 | Cumple |
| 156 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 49,123.67 | 0 | 0.0705452 | Cumple |
| 367 | Contraviento_2 | ORC 114 x 6.4 | SISMO_DOWN_1 | 49,039.64 | 0 | 0.0704245 | Cumple |
| 168 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 48,688.83 | 0 | 0.0699207 | Cumple |
| 356 | Contraviento_2 | ORC 114 x 6.4 | SISMO_DOWN_1 | 47,507.70 | 0 | 0.0682246 | Cumple |
| 355 | Contraviento_2 | ORC 114 x 6.4 | SISMO_DOWN_1 | 47,477.30 | 0 | 0.0681809 | Cumple |
| 167 | Contraviento_1 | ORC 114 x 6.4 | SISMO_DOWN_1 | 46,807.95 | 0 | 0.0672197 | Cumple |

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
