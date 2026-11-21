# Estructura de la documentación

Objetivo: que cualquier lector encuentre lo que busca en dos clics y que cada página tenga un solo propósito.

## Árbol `/docs` recomendado

```
README.md                      # puerta de entrada (plantilla en plantillas.md)
AGENTS.md                      # opcional: guía corta para asistentes de código (build, test, convenciones, qué evitar)
CONTRIBUTING.md  SECURITY.md  CHANGELOG.md  LICENSE
docs/
  index.md                     # mapa + a quién va dirigida cada parte
  VERIFICATION.md              # qué se ejecutó, dónde, cuándo y resultado
  tutorials/                   # aprender: primeros-pasos.md
  how-to/                      # tareas técnicas: desplegar.md, agregar-endpoint.md, restaurar-backup.md
  reference/                   # hechos
    api/ (openapi.yaml, autenticacion.md, errores.md)
    base-de-datos/ (diccionario.md, er.md)
    configuracion/variables-entorno.md
    cli-y-scripts.md
  explanation/                 # el porqué
    arquitectura/ (contexto.md, contenedores.md, despliegue.md)
    estructura-del-repo.md
    archivos-clave.md
    seguridad.md
  adr/                         # 0000-usar-adr.md, 0001-...md, template.md
  backend/  frontend/  admin/  mobile/  infra/    # una carpeta por capa EXISTENTE, cada una con README
  runbooks/                    # despliegue-y-rollback.md, backup-restauracion.md, incidente-<x>.md
  user-manual/                 # manual de usuario (ver manual-de-usuario.md)
    index.md  primeros-pasos.md  tareas/  faq.md  solucion-de-problemas.md  glosario.md  contacto.md
    assets/screenshots/<flujo>/NN-<flujo>-<paso>.png
  diagrams/src/*.mmd           # fuentes de diagramas
  assets/screenshots.yml       # manifiesto captura -> paso -> script
  tools/                       # scripts: docs:screenshots, docs:check, docs:build
```

Repos pequeños: versión plana `docs/{arquitectura,api,bd,manual,runbooks,adr}/`, conservando un tipo de contenido por archivo.

**Por qué esta forma:** separa por tipo de lectura (Diátaxis), no por departamento. Una persona con prisa busca "cómo hago X" (how-to), otra busca "qué campos tiene Y" (referencia). Mezclarlos obliga a leer todo para encontrar una línea. Las carpetas por capa evitan que el backend, el móvil y la infraestructura se disuelvan en un solo documento gigante.

## Diátaxis (cómo clasificar cada párrafo)

| Tipo | Sirve para | Regla | Ejemplo |
|---|---|---|---|
| Tutorial | Aprender | Camino único, resultado concreto, sin opciones ni teoría | "Levanta el proyecto y crea tu primer pedido" |
| How-to | Resolver una tarea | Receta por objetivo, sin explicar de más | "Cómo rotar la clave de firma" |
| Referencia | Consultar hechos | Seca, exacta, completa, estructurada como el código | Endpoints, variables, esquema BD |
| Explicación | Entender el porqué | Contexto, alternativas, decisiones | "Por qué separamos colas y API" |

- [ ] Antes de escribir un párrafo, clasifícalo. Si mezcla instrucciones y teoría, divídelo y enlaza.
- [ ] El manual de usuario es sobre todo how-to más un tutorial de inicio.

## C4 (diagramas de arquitectura)

- Nivel 1 **Contexto**: sistema, usuarios y sistemas externos. Siempre.
- Nivel 2 **Contenedores**: apps desplegables, procesos, BD, colas, con tecnología y protocolo. Siempre.
- Nivel 3 **Componentes**: interior de un contenedor. Solo si aporta.
- Nivel 4 **Código**: casi nunca; se genera del código.
- Extras: despliegue y secuencia (dinámico) cuando importan.
- Máximo 6-8 elementos y ~6 relaciones por diagrama; cada flecha con verbo y protocolo ("envía pedidos / HTTPS JSON").

## ADR (registro de decisiones)

- Un archivo por decisión en `docs/adr/`, numerado `0001-titulo.md`; inmutable una vez aceptado (se reemplaza con otro ADR que lo "supersede").
- Campos: título, estado, contexto, factores, opciones, decisión, consecuencias, confirmación (plantilla en `plantillas.md`).
- **Proyecto heredado:** reconstruye ADR desde historial de git, dependencias y estructura, y márcalos `Estado: reconstruido (inferido)` para no fingir certeza.
- Diferencia clave: lo que **impone el framework** no necesita ADR; lo que **decidimos nosotros** sí.

## arc42 ligero

Las 12 secciones de arc42 son un menú, no una obligación. Para proyectos pequeños y medianos usa solo:

| Sección arc42 | Dónde vive en `/docs` |
|---|---|
| 1 Introducción y metas | README + `explanation/arquitectura/contexto.md` |
| 3 Contexto y alcance | C4 nivel 1 |
| 5 Vista de bloques | C4 nivel 2-3, `explanation/estructura-del-repo.md` |
| 6 Vista de ejecución | secuencias de los flujos clave |
| 7 Vista de despliegue | `explanation/arquitectura/despliegue.md`, `infra/` |
| 8 Conceptos transversales | `explanation/seguridad.md`, errores, logging, i18n |
| 9 Decisiones | `adr/` |
| 12 Glosario | `user-manual/glosario.md` (compartido) |

Añade 2 (restricciones), 10 (calidad) y 11 (riesgos y deuda) solo si el cliente las pide.

## Cabecera común de cada documento

```yaml
---
title: Crear un pedido
tipo: how-to            # tutorial | how-to | referencia | explicacion
audiencia: operador     # desarrollo | operacion | usuario-final | cliente
ultima_verificacion: AAAA-MM-DD
verificado_contra: commit abc1234 / v1.4.0
verificado_con: navegador web en entorno de pruebas
estado: verificado      # verificado | parcial | sin-verificar
---
```

- [ ] `estado` coincide con la fila correspondiente de `VERIFICATION.md`.
- [ ] Autor/propietario = la empresa o persona responsable, nunca una herramienta.

## Documentos para asistentes (opcional)

`AGENTS.md` = README corto para asistentes de código: cómo compilar, probar, convenciones, qué no tocar. Mantenerlo separado del README humano. `llms.txt` = índice de contenido clave (formato **verificar en el entorno** antes de adoptarlo). Ninguno lleva atribución de autoría a herramientas.
