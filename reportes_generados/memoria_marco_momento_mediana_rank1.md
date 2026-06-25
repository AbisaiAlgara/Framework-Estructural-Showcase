# Memoria de Calculo Estructural

**Fecha de generacion:** 2026-06-04 11:46:00

## 1. Alcance

La presente memoria documenta el modelo estructural seleccionado, la metodologia de analisis y las verificaciones normativas implementadas en el framework. Los resultados se recalculan con el motor validado antes de generar este documento.

## 2. Modelo Analizado

| Parametro | Valor |
| --- | --- |
| Nodos | 100 |
| Elementos | 184 |
| Niveles | 4 |
| Altura total (m) | 12.8000 |
| Peso de acero (kg) | 56,517.97 |
| Masa sismica (kg) | 648,064.38 |

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
| Masa propia elementos (kg) | 56,517.97 |
| Masa sismica total (kg) | 648,064.38 |
| Peso sismico total (N) | 6,357,511.60 |

### 4.1 Masa por Nivel

| Nivel (m) | CM (kgf) | CV_INST (kgf) | Masa propia (kg) | Masa sismica (kg) |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 3,200.00 | 3,200.00 |
| 3.2000 | 127,052.69 | 27,323.16 | 15,231.58 | 169,607.44 |
| 6.4000 | 127,052.69 | 27,323.16 | 15,231.58 | 169,607.44 |
| 9.6000 | 127,052.69 | 27,323.16 | 15,231.58 | 169,607.44 |
| 12.8000 | 109,292.64 | 19,126.21 | 7,623.21 | 136,042.07 |

## 5. Analisis Modal

El numero de modos se selecciona buscando alcanzar la participacion modal minima configurada.

| Parametro | Valor |
| --- | --- |
| Objetivo participacion | 0.9 |
| Modos usados | 7 |
| Maximo modos |  |

### 5.1 Periodos

| Modo | Periodo (s) |
| --- | --- |
| 1 | 0.874992 |
| 2 | 0.862123 |
| 3 | 0.799297 |
| 4 | 0.269424 |
| 5 | 0.261502 |
| 6 | 0.252734 |
| 7 | 0.241505 |

### 5.2 Participacion Modal X

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.587848 |
| 2 |  |  | 0.747742 |
| 3 |  |  | 0.74878 |
| 4 |  |  | 0.750981 |
| 5 |  |  | 0.767192 |
| 6 |  |  | 0.910505 |
| 7 |  |  | 0.910986 |

### 5.2 Participacion Modal Y

| Modo | Masa modal | Participacion | Acumulado |
| --- | --- | --- | --- |
| 1 |  |  | 0.0269601 |
| 2 |  |  | 0.168528 |
| 3 |  |  | 0.752495 |
| 4 |  |  | 0.784798 |
| 5 |  |  | 0.786018 |
| 6 |  |  | 0.786985 |
| 7 |  |  | 0.910242 |

## 6. Resumen Normativo

| Criterio | Calculado | Limite | Estado |
| --- | --- | --- | --- |
| D/C max | 0.810445 | 1.0000 | Cumple |
| Deriva servicio | 0.00218713 | 0.005 | Cumple |
| Deriva colapso | 0.0097206 | 0.01 | Cumple |
| Desp. azotea | 0.0227275 | 0.1 | Cumple |
| KL/r | 64.4545 | 200.0000 | Cumple |
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
| Nivel 3.2 m | SISMO_DOWN_1 | 0.000975589 | 0.00433595 |
| Nivel 6.4 m | SISMO_DOWN_1 | 0.00188681 | 0.00838581 |
| Nivel 9.600000000000001 m | SISMO_DOWN_1 | 0.0020528 | 0.00912355 |
| Nivel 12.8 m | SISMO_DOWN_1 | 0.00218713 | 0.0097206 |

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
| Perfil | IR 610 x 125.0 |
| Combinacion critica | SISMO_DOWN_1 |
| Longitud L (m) | 3.2000 |
| Area A (m2) | 0.01594 |
| Ix (m4) | 0.00098647 |
| Iy (m4) | 3.929e-05 |
| r_min (m) | 0.0496474 |
| KL/r | 64.4545 |
| Pu (kN) | 59.0603 |
| Mu (kN-m) | 216.1795 |
| My local (kN-m) | 216.1795 |
| Mz local (kN-m) | 0.00415648 |
| phi Pn (kN) | 3,651.11 |
| phi Mny (kN-m) | 1,139.26 |
| phi Mnz (kN-m) | 165.7212 |
| Extremo critico | i |
| Tipo revision | IR biaxial: My local contra eje fuerte; Mz local contra eje debil |

Sustitucion numerica:

- `Pu/phiPn = 59.060 / 3651.107 = 0.0162`
- `My/phiMny = 216.180 / 1139.256 = 0.1898`
- `Mz/phiMnz = 0.004 / 165.721 = 0.0000`
- `Suma flexion biaxial = 0.1898`
- Rama usada: `Pu/phiPn < 0.20: Pu/(2 phiPn) + My/phiMny + Mz/phiMnz`
- `D/C = 0.1979`
- Estado: **Cumple**

### 9.3 Resumen de Columnas Criticas

| Elem | Grupo | Perfil | Combo | KL/r | Pu (N) | My (N-m) | Mz (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 104 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 59,060.28 | 216,179.55 | 4.1565 | 0.197868 | Cumple |
| 101 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 56,640.06 | 213,478.87 | 4.1565 | 0.195166 | Cumple |
| 108 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 48,850.19 | 179,250.76 | 4.1565 | 0.164055 | Cumple |
| 105 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 49,089.39 | 179,059.88 | 4.1565 | 0.16392 | Cumple |
| 112 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 40,710.37 | 146,289.01 | 4.1565 | 0.134008 | Cumple |
| 109 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 40,215.24 | 145,357.61 | 4.1565 | 0.133122 | Cumple |
| 116 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 32,778.10 | 116,776.88 | 4.1565 | 0.107017 | Cumple |
| 113 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 32,832.54 | 116,577.94 | 4.1565 | 0.106849 | Cumple |
| 204 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 43,709.31 | 98,192.65 | 7.8760 | 0.0922235 | Cumple |
| 201 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 39,395.90 | 90,995.91 | 7.8760 | 0.0853157 | Cumple |
| 205 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 35,620.01 | 80,579.47 | 7.8760 | 0.0756554 | Cumple |
| 208 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 35,628.87 | 80,239.85 | 7.8760 | 0.0753585 | Cumple |
| 212 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 30,260.72 | 66,963.83 | 7.8760 | 0.0629701 | Cumple |
| 209 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 29,022.90 | 65,308.12 | 7.8760 | 0.0613473 | Cumple |
| 304 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 28,243.87 | 56,830.91 | 8.3225 | 0.0538023 | Cumple |
| 406 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 28,373.45 | 56,146.84 | 8.6554 | 0.0532216 | Cumple |
| 216 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 24,247.24 | 53,331.80 | 7.8760 | 0.0501809 | Cumple |
| 213 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 24,133.30 | 53,111.70 | 7.8760 | 0.0499721 | Cumple |
| 301 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 24,904.95 | 52,766.27 | 8.3225 | 0.0497773 | Cumple |
| 404 | Columna_1 | IR 610 x 125.0 | SISMO_DOWN_1 | 64.4545 | 19,717.09 | 53,485.41 | 8.6554 | 0.0497001 | Cumple |

## 10. Resumen de Elementos

### 10.1 Vigas Criticas

| Elem | Grupo | Perfil | Combo | Mu (N-m) | phi Mn (N-m) | D/C | Estado |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 426 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 61,872.17 | 76,343.48 | 0.810445 | Cumple |
| 432 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 52,137.73 | 76,343.48 | 0.682936 | Cumple |
| 427 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 47,926.17 | 76,343.48 | 0.62777 | Cumple |
| 433 | Viga_2 | IR 152 x 29.8 | GRAV_DISEÑO | 40,648.94 | 76,343.48 | 0.532448 | Cumple |
| 321 | Viga_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 85,440.96 | 371,476.20 | 0.230004 | Cumple |
| 221 | Viga_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 84,567.13 | 371,476.20 | 0.227652 | Cumple |
| 224 | Viga_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 78,358.65 | 371,476.20 | 0.210939 | Cumple |
| 229 | Viga_1 | IR 406 x 59.5 | SISMO_DOWN_1 | 76,209.20 | 371,476.20 | 0.205152 | Cumple |
| 226 | Viga_1 | IR 406 x 59.5 | GRAV_DISEÑO | 75,115.71 | 371,476.20 | 0.202209 | Cumple |
| 227 | Viga_1 | IR 406 x 59.5 | GRAV_DISEÑO | 75,082.81 | 371,476.20 | 0.20212 | Cumple |
| 126 | Viga_1 | IR 406 x 59.5 | GRAV_DISEÑO | 74,990.96 | 371,476.20 | 0.201873 | Cumple |
| 127 | Viga_1 | IR 406 x 59.5 | GRAV_DISEÑO | 74,957.33 | 371,476.20 | 0.201782 | Cumple |

### 10.2 Contravientos Criticos

_Sin datos disponibles._

## 11. Dictamen Automatico

**Estado global:** CUMPLE

El modelo analizado cumple con los criterios automaticos revisados por el framework para el alcance documentado.

Nota: esta memoria es una ayuda de trazabilidad y no sustituye la revision profesional del ingeniero responsable.
