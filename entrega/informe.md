# Informe técnico del taller

## BPMN
_Taller 1 - BPMN_

## Integrantes del equipo
- Esteban Fernando Forero Montejo
- Santiago Sabogal Millan (santiagoSaMi)
- Carlos David Cruz Pavas
- Juan Felipe Cepeda Uribe

## Descripción general del trabajo
Para la entrega final se modelo el proceso real del cliente "Direccion de Desarrollo Estrategico - Universidad de La Sabana", enfocado en la actualizacion de la *Encuesta de Autoevaluacion Institucional por Programas* con base en lineamientos del CNA.

El objetivo del modelo fue representar de forma clara:
- El flujo manual actual del proceso.
- Las interacciones entre actor interno y proveedor externo.
- Los puntos de decision y control de calidad que generan mayor riesgo operativo.
- Los artefactos documentales criticos (PDF CNA, Excel historico y Excel final con trazabilidad).

## Proceso de desarrollo
1. Se levanto el contexto del cliente en reunion con la responsable del proceso.
2. Se definio el alcance del modelado: actualizacion del banco de preguntas y validacion de links de encuesta.
3. Se identificaron actores, actividades, documentos y decisiones.
4. Se elaboro el modelo BPMN final con colaboracion entre "Encargado" y "Proveedor Externo".
5. Se valido coherencia del flujo con las buenas practicas BPMN revisadas en la investigacion.

## Análisis del modelo propuesto
### Flujo principal (happy path)
1. El encargado recibe lineamientos CNA y abre el consolidado historico en Excel.
2. Compara manualmente PDF vs Excel.
3. Si hay cambios, marca preguntas nuevas/modificadas/eliminadas y revisa consolidado con su jefe.
4. Envia Excel final al proveedor.
5. El proveedor actualiza su banco de preguntas, genera links personalizados y los envia.
6. El encargado verifica links uno por uno.
7. Si los links son correctos, el proceso finaliza como "Proceso listo".

### Flujos alternos
- Si no hay cambios despues de la comparacion, se notifica al proveedor sin cambios y el proceso termina en "Fin sin cambios".
- Si los links presentan errores, se reenvia al proveedor para correccion y se repite el ciclo de verificacion.

### Puntos criticos del proceso
- Comparacion manual PDF vs Excel: alto consumo de tiempo y riesgo de omision.
- Revision conjunta prolongada: dependencia de pocas personas.
- Verificacion de links: control de calidad obligatorio para evitar errores en despliegue.

### Diferencias frente al caso base de clase
- Caso base (Clinica Salud Viva): orientado a servicio al paciente (agendamiento).
- Caso cliente real: colaboracion inter-organizacional con trazabilidad documental y control de cambios normativos.
- Se conserva la estructura BPMN (evento inicio, actividades, gateways, interaccion y eventos de fin), pero cambia el dominio y el tipo de decisiones de negocio.

## Diagrama final entregado
- Archivo BPMN editable: `entrega/diagram-client.bpmn`
- El diagrama modela dos participantes:
  - Encargado
  - Proveedor Externo
 
<img width="3100" height="562" alt="image" src="https://github.com/user-attachments/assets/e0627ee5-9fb2-4e21-aaaf-f8d90b491585" />


## Tabla de actores, entidades o componentes
| Tipo | Elemento | Rol en el proceso |
|------|----------|-------------------|
| Actor interno | Encargado | Ejecuta la comparacion, marcacion de cambios, revision, envio y verificacion final. |
| Actor externo | Proveedor Externo | Actualiza banco de preguntas, genera links y responde ajustes. |
| Documento | PDF lineamientos CNA | Fuente normativa para actualizar preguntas. |
| Documento | Excel consolidado historico | Base de comparacion de preguntas previas. |
| Documento | Excel final con trazabilidad de cambios | Entregable oficial al proveedor para actualizacion. |
| Decision | ¿Hay cambios? | Define si se continua con actualizacion o cierre sin cambios. |
| Decision | ¿Links correctos? | Define cierre exitoso o reproceso con proveedor. |

## Investigacion complementaria: Buenas practicas BPMN aplicadas

El estandar **Business Process Model and Notation (BPMN 2.0)**, mantenido por el Object Management Group (OMG, 2014), representa la sintesis de las mejores practicas de la comunidad de modelado de procesos. Su objetivo principal es crear diagramas que sean comprensibles tanto para analistas de negocio, como para desarrolladores y usuarios finales, sin necesidad de documentacion adicional (Silver, 2012).

A partir de fuentes autorizadas (OMG, Silver, Bizagi, Camunda y estudios academicos), se identificaron las siguientes buenas practicas clave que se aplicaron rigurosamente en el modelo del proceso real del cliente:

1. **Inicio y fin explicitos con eventos claros** (Bizagi; Camunda)  
   Todo proceso debe comenzar con al menos un evento de inicio y terminar con uno o mas eventos de fin que indiquen el resultado de negocio. En nuestro diagrama se utilizo un evento de inicio al recibir el PDF de lineamientos CNA y dos eventos de fin diferenciados: uno para "Proceso listo" (links verificados correctamente) y otro para "Fin sin cambios" (notificacion al proveedor). Esto evita ambiguedades y facilita la identificacion inmediata del alcance.

2. **Flujo logico de izquierda a derecha y sin cruces** (Camunda; Bizagi)  
   La secuencia se organizo cronologicamente de izquierda a derecha, respetando la direccion natural de lectura. Se evito cualquier cruce de flujos de secuencia o mensaje, reorganizando los elementos para mantener una lectura limpia.

3. **Separacion clara de roles mediante pools y lanes (colaboracion)** (OMG; Camunda)  
   Se modelaron dos participantes diferenciados: "Encargado" (actor interno) y "Proveedor Externo". Esto refleja fielmente la interaccion entre la institucion y su aliado tecnologico, mostrando flujos de mensaje para el envio del Excel consolidado y la recepcion de links personalizados.

4. **Etiquetado estricto y orientado a negocio** (Silver; Bizagi; Camunda)  
   - Actividades: verbo + objeto ("Recibir PDF lineamientos CNA", "Comparar PDF CNA vs Excel", "Marcar preguntas nuevas en azul").
   - Gateways exclusivos: formulados como pregunta ("¿Hay cambios?", "¿Links correctos?").
   - Eventos de fin: nombran el estado final de negocio ("Proceso listo", "Fin sin cambios").
   Esta convencion garantiza que cualquier lector entienda el proceso sin requerir leyendas.

5. **Uso de objetos de datos y asociaciones** (OMG; Bizagi)  
   Se asociaron explicitamente los documentos criticos: "[PDF Lineamientos CNA]", "[Excel Consolidado Historico]" y "[Excel Final con Trazabilidad de Cambios]" a las tareas correspondientes.

6. **Gateways exclusivos para decisiones y excepciones** (Camunda; Pufahl et al., 2022)  
   Se emplearon gateways XOR en los dos puntos: deteccion de cambios y verificacion de links. El "no" en el primer gateway lleva al fin "sin cambios", y el "no" en el segundo crea un ciclo de correccion con el proveedor.

7. **Simplicidad y enfasis en el flujo principal** (Camunda; Silver)  
   El camino principal se mantiene lineal y legible: cambios detectados -> trazabilidad -> envio al proveedor -> generacion de links -> verificacion -> cierre.

**Aplicacion al cliente y diferencias con el caso base**  
El caso base de la Clinica Salud Viva (agendamiento de citas) es un proceso orientado al paciente con seleccion secuencial. En contraste, el proceso del cliente se centra en comparacion documental, trazabilidad de cambios y coordinacion con proveedor externo.  

Aun asi, se conserva la estructura BPMN esencial: evento de inicio -> actividades -> decisiones -> interaccion entre participantes -> evento(s) de fin.
