# Reporte de criterios de constructibilidad usados

## Alcance

Este reporte documenta los criterios de constructibilidad aplicados por el framework durante la optimizacion genetica y la validacion de candidatos. Los criterios se aplican sobre grupos de diseno, perfiles de catalogo y geometria real del edificio.

El objetivo es evitar soluciones numericamente optimas pero poco construibles, por ejemplo perfiles demasiado delgados, incompatibles en nudos, cambios de columna en altura o jerarquias verticales poco razonables.

## Fuentes dentro del framework

- Motor de penalizacion: `optimization/constructibilidad.py`.
- Reparacion genetica previa al analisis: `optimization/genotipo.py`.
- Criterio CFVD por sistema lateral: `optimization/objetivo_deriva.py`.
- Evidencia del edificio ganador: `reportes_generados/memoria_chevron_x_grande_final_rank52_num12_evidencia.json`.

## Criterios implementados

### 1. Espesor minimo de placas y paredes

Se exige un espesor minimo de `3/16 in`, equivalente a `0.0047625 m`.

Aplica a:

- `tw` en perfiles IR.
- `tf` en perfiles IR.
- `t` en perfiles OR.
- `t` en contravientos OR.

Si un perfil tiene espesor menor al minimo, el candidato recibe penalizacion de constructibilidad. Ademas, el genotipo intenta reparar el individuo seleccionando el perfil valido mas cercano dentro del catalogo.

Penalizacion usada:

- `PENAL_ESPESOR = 50.0`.

### 2. Columnas con mismo perfil en toda la altura

En una misma linea vertical de columnas no se permite cambiar de perfil en altura.

La linea vertical se identifica por la posicion real `(x, y)` de cada columna, no por el nombre del grupo. Esto evita soluciones donde una columna cambia de familia o seccion entre pisos aunque tenga continuidad geometrica.

La regla responde al criterio acordado: las columnas pueden variar por grupo de optimizacion, pero no pueden cambiar en altura dentro de una misma linea vertical.

Penalizacion usada:

- `PENAL_COLUMNA_CAMBIO_ALTURA = 1000.0`.

Esta penalizacion es deliberadamente alta porque se considera una violacion fuerte de constructibilidad.

### 3. Jerarquia vertical telescopica en vigas y contravientos

Para vigas y contravientos, los grupos superiores no deben exceder dimensionalmente a los grupos inferiores cuando existe jerarquia vertical real.

El framework usa la coordenada `z_ref` promedio de los grupos para ordenar niveles. No depende del sufijo del nombre del grupo.

Dimensiones revisadas:

- `d`
- `bf`
- `tw`
- `tf`
- `t`

Interpretacion:

- En niveles superiores, las dimensiones no deben ser mayores que las del nivel inferior comparable.
- El criterio evita que el edificio crezca en seccion hacia arriba sin justificacion constructiva.

Penalizacion usada:

- `PENAL_TELESCOPICO = 25.0`.

### 4. Compatibilidad de ancho en nudos

Se revisa que vigas y contravientos conectados a columnas no excedan los limites geometricos de la columna concurrente.

Para vigas:

- Se controla principalmente el ancho de patin `bf`.
- Si la columna es OR, el `bf` de la viga debe caber dentro del ancho de la columna.
- Si la columna es IR, el `bf` de la viga debe caber simultaneamente dentro del `bf` y el `d` de la columna.

Para contravientos:

- Se controla el mayor valor entre `d` y `bf` del contraviento.
- Si la columna es OR, el contraviento debe caber dentro del ancho de la columna.
- Si la columna es IR, debe caber dentro de `bf` y `d` de la columna.

Penalizaciones usadas:

- `PENAL_ANCHO_NUDO = 25.0` para vigas.
- `PENAL_CONTRA_ANCHO = 25.0` para contravientos.

### 5. Columna fuerte - viga debil

El criterio CFVD se aplica automaticamente solo cuando el sistema lateral detectado es `marco_momento`.

Para sistemas arriostrados se omite por defecto, porque el mecanismo lateral principal no depende de la jerarquia flexionante viga-columna del marco.

Reglas:

- Marco a momento: CFVD activo.
- Sistema arriostrado: CFVD omitido por defecto.
- Puede forzarse en arriostrados mediante configuracion, pero no fue el criterio usado en la comparativa final.

Factor usado:

- `CFVD_Factor = 1.2`.

Penalizacion usada:

- `PENAL_CFVD = 15.0`.

En el edificio ganador `Chevron en X`, CFVD queda registrado como:

- `CFVD_Aplicado = false`
- `CFVD_Criterio_Aplicacion = omitido_por_sistema_arriostrado`

### 6. Control de familia de perfil

El framework permite o restringe el cambio de familia segun la configuracion:

- `Permitir_Cambio_Familia_Perfil = SI`: columnas pueden elegir entre IR, OR y OR mixtos; vigas usan IR; contravientos usan OR.
- Si estuviera desactivado, cada grupo quedaria restringido a la familia de entrada.

En la comparativa final se uso:

- `Permitir_Cambio_Familia_Perfil = SI`.

Por eso el optimizador pudo elegir familias distintas cuando eran compatibles con el grupo y con las reglas de nudo, altura y espesor.

### 7. Reparacion genetica antes del analisis

Antes de enviar un individuo al motor estructural, el genotipo intenta reparar automaticamente:

- Indices fuera de catalogo.
- Espesores menores al minimo.
- Cambios de columna en altura.
- Telescopia vertical en vigas y contravientos.
- Anchos incompatibles en nudos.
- CFVD cuando aplica.

Esto reduce candidatos inviables antes de gastar tiempo computacional en OpenSees.

### 8. Guillotina por inviabilidad

Si la penalizacion constructiva o de embudo supera umbrales de inviabilidad, el candidato puede descartarse mediante una fitness de guillotina sin ejecutar analisis estructural completo.

Esto evita que soluciones claramente no construibles contaminen el frente Pareto o consuman costo computacional.

## Resultado para el sistema ganador

Candidato ganador:

- Sistema: `Chevron en X`
- Corrida: `chevron_x_grande_dc085_final`
- Rank: `52`
- Memoria: `memoria_chevron_x_grande_final_rank52_num12`

Perfiles:

| Grupo | Perfil |
|---|---|
| Columna_1 | ORC 229 x 6.4 |
| Columna_2 | ORC 229 x 6.4 |
| Viga_1 | IR 305 x 20.8 |
| Contraviento_1 | ORC 114 x 6.4 |
| Contraviento_2 | ORC 102 x 5.3 |
| Viga_2 | IR 305 x 20.8 |

Validacion de constructibilidad:

| Criterio | Estado |
|---|---|
| Constructibilidad total | Cumple |
| Espesor minimo | Cumple |
| Columna mismo perfil en altura | Cumple |
| Telescopia vertical | Cumple |
| Ancho de nudo | Cumple |
| CFVD | Cumple por omision justificada en sistema arriostrado |
| Guillotina | No activada |

Penalizaciones registradas:

| Penalizacion | Valor |
|---|---:|
| Columna mismo perfil en altura | 0.0 |
| Telescopico | 0.0 |
| Ancho de nudo | 0.0 |
| Total | 0.0 |

## Que no sustituye este reporte

Estos criterios no sustituyen:

- Diseno LRFD formal de elementos.
- Diseno de conexiones.
- Revision local de placas, soldaduras y pernos.
- Revision de pandeo local detallado mas alla de propiedades de catalogo.
- Coordinacion arquitectonica final con claros, fachadas, muros, instalaciones y accesos.

La funcion del filtro de constructibilidad es controlar la viabilidad geometrica y logica del espacio de busqueda del optimizador.

## Recomendacion para memoria final

Para la memoria de calculo del edificio ganador conviene incluir estos criterios como una seccion previa a la revision LRFD, dejando claro que:

- Fueron usados como restricciones del optimizador.
- El candidato ganador tiene penalizacion constructiva total igual a cero.
- La seleccion final debe verificarse despues con diseno detallado de conexiones y requisitos LRFD completos.
