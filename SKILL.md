---
name: documentacion
description: Flujo completo para documentar un proyecto de software de punta a punta - estudio del código, levantarlo y probarlo de verdad, documentación técnica por capa (backend, frontend, API, base de datos, panel admin, móvil, infraestructura) con flujos y diagramas, y manual de usuario con capturas reales. Úsalo SIEMPRE que se pida documentar, documentación, "documenta este proyecto", manual de usuario, guía de uso, README, arquitectura del proyecto, diagrama, flujos, capturas de pantalla, onboarding de nuevos integrantes, explicar el proyecto, por qué está ordenado así, archivos más importantes, handoff o entrega al cliente, runbook, ADR, diccionario de datos, OpenAPI — aunque el pedido no diga "skill" ni "documentación" explícitamente.
---

# Documentación de proyectos

Esta skill convierte "documenta este proyecto" en un proceso repetible. La documentación **se deriva del código real y de pruebas reales**; lo que no se ejecutó es una hipótesis y se marca como tal.

## Reglas obligatorias

1. **Derivar, no inventar.** Endpoints desde el código o el OpenAPI, tablas desde el esquema vivo, variables desde `.env.example`, comandos desde los scripts del manifiesto.
2. **Cada paso del manual se ejecuta antes de escribirse.** Lo que no se pudo probar se marca `> Sin verificar` en el texto y en `docs/VERIFICATION.md`. Los comandos de esta skill tomados de la investigación previa (adb, simctl, Playwright, herramientas de lint) llevan la etiqueta **verificar en el entorno** hasta que los corras.
3. **Explicar el porqué**, no solo el qué: por qué el repo está ordenado así, cuáles son los archivos más importantes y por qué. Si el motivo no consta en código, commits o ADR, escribe "motivo no documentado; hipótesis: ..." y márcalo.
4. **Un tipo de contenido por página** (Diátaxis): tutorial, how-to, referencia o explicación. No mezclarlos.
5. **Datos demo siempre.** Nunca capturar ni citar datos reales de clientes, credenciales, tokens, llaves, IPs de servidores ni barras de dirección con sesión. Los accesos se entregan por canal seguro aparte, nunca dentro del documento.
6. **Autoría clara.** El autor de lo entregado es la persona o empresa responsable. Antes de entregar, revisa que README, docs, metadatos, comentarios, capturas y mensajes de commit no lleven nombres de herramientas, datos internos ni líneas de coautoría que la política del proyecto no permita.
7. **Idioma = el del mercado del cliente.** Esta skill está en español, pero el documento entregado sigue al cliente (p. ej. es-CL tuteo neutro, pt-BR, inglés EE. UU.). Ver `references/manual-de-usuario.md` y la nota sobre proyectos dirigidos a EE. UU.
8. **No sobre-documentar.** Prioriza lo que un integrante nuevo o un usuario final necesita para entender y usar el sistema. Si una página no tiene lector claro, no se escribe.
9. **Entornos seguros.** Probar solo en desarrollo o pruebas, nunca contra producción. Limpiar los datos de ensayo por el mismo camino que los creó.
10. **Estados externos con evidencia fechada.** No afirmes "aprobado/pendiente/desplegado" (tiendas, dominios, certificados) sin consultar la fuente.

## Fases

### Fase 1 — Estudio del código (`references/fase-estudio.md`)
- [ ] Inventario: manifiestos, entradas, rutas, modelos, variables, scripts, CI, Docker.
- [ ] Detectar qué capas existen (backend, API, BD, web, admin, móvil, infra).
- [ ] Trazar 2 o 3 flujos de extremo a extremo (desde donde comienza hasta donde termina).
- [ ] Elegir los archivos más importantes con criterio objetivo y explicar por qué.
- [ ] Redactar el plan: lista de documentos, audiencia y tipo por cada uno. Pregunta solo lo que no se pueda deducir (público, idioma, nivel de detalle).

### Fase 2 — Levantar y probar el sistema (`references/capturas-y-pruebas.md`)
- [ ] Entorno limpio con datos demo (seed dedicado). Registrar SO, versiones y commit.
- [ ] Ejecutar el README tal cual; cada fallo corrige el doc o abre un bug.
- [ ] Según la máquina: **Mac** = simulador iOS o Android; **Windows** = emulador Android o navegador web; WSL2/servidor = ver el archivo.
- [ ] Para pruebas funcionales amplias reutiliza la skill `qa`; aquí solo se recorre para documentar.
- [ ] Abrir `docs/VERIFICATION.md` desde el primer día y llenarlo mientras pruebas.

### Fase 3 — Documentación técnica por capa (`references/documentacion-por-capa.md`, `references/diagramas.md`)
- [ ] Una carpeta por capa existente; no crear carpetas vacías para capas que no hay.
- [ ] Diagramas Mermaid (contexto, contenedores, secuencia de flujos clave, ER, despliegue), máximo 6-8 elementos cada uno.
- [ ] Base de datos: diccionario de datos y ER derivados del esquema vivo (consulta de solo lectura); no dupliques las reglas de modelado de la guía de base de datos del proyecto.
- [ ] Backend: decisiones de arquitectura según la skill `arquitectura-backend`; aquí solo se documenta lo que existe.
- [ ] ADR retroactivos marcados `reconstruido (inferido)`.

### Fase 4 — Manual de usuario con capturas (`references/manual-de-usuario.md`)
- [ ] Qué es, por qué y cuándo usarlo (y cuándo no), para quién, antes de empezar.
- [ ] Una tarea = una página: objetivo, pasos numerados con captura, resultado esperado, qué hacer si falla.
- [ ] FAQ, solución de problemas (síntoma, causa, solución, con el texto exacto del mensaje), glosario.
- [ ] Capturas reales, datos demo, mismo tamaño y tema, manifiesto captura → paso → script.

### Fase 5 — Verificación y control de calidad (`references/calidad-y-mantenimiento.md`)
- [ ] Reejecutar los pasos del manual siguiendo solo el texto escrito.
- [ ] Linter de Markdown, enlaces, diagramas renderizan, búsqueda de secretos y de menciones prohibidas en `docs/` y capturas.
- [ ] Cobertura: módulos, rutas y tablas del código frente a lo documentado.
- [ ] Cerrar `VERIFICATION.md` con lo probado y lo **sin verificar**, con motivo.

### Fase 6 — Entrega
- [ ] Fuente única en Markdown dentro del repo; PDF, DOCX o sitio se generan desde ella.
- [ ] Si el cliente pide PDF o DOCX, usa la skill de documentos correspondiente del sistema (docx o pdf) y `to-md` para convertir insumos existentes. **Verificar disponibilidad** antes de prometerlo.
- [ ] Informe final: qué se documentó, qué se verificó, qué quedó sin verificar, riesgos y deuda de documentación.
- [ ] Commits con la identidad acordada del proyecto y sin menciones que su política no permita (ver `references/calidad-y-mantenimiento.md`).

## Qué entrega esta skill (mínimo)

| Entregable | Audiencia | Ruta recomendada |
|---|---|---|
| README con arranque probado | Desarrollo | `README.md` |
| Mapa de documentación | Todos | `docs/index.md` |
| Arquitectura, estructura y archivos clave con su porqué | Desarrollo | `docs/explanation/` |
| Referencias por capa (API, BD, config, infra) | Desarrollo y operación | `docs/reference/`, `docs/<capa>/` |
| Runbooks y how-to | Operación | `docs/runbooks/`, `docs/how-to/` |
| Manual de usuario con capturas | Usuario final | `docs/user-manual/` |
| Registro de verificación | Revisión y cliente | `docs/VERIFICATION.md` |

## Índice de references

- `references/estructura-de-docs.md` — árbol `/docs`, Diátaxis, C4, ADR, arc42 ligero, cabecera común.
- `references/fase-estudio.md` — cómo leer un proyecto ajeno y decidir los archivos más importantes.
- `references/documentacion-por-capa.md` — qué documentar y plantillas por capa.
- `references/diagramas.md` — Mermaid con ejemplos mínimos y reglas.
- `references/manual-de-usuario.md` — estructura, escritura para no técnicos, glosario, FAQ, idioma.
- `references/capturas-y-pruebas.md` — levantar y recorrer el sistema por entorno, capturas, `VERIFICATION.md`, trampas reales.
- `references/calidad-y-mantenimiento.md` — checklist, desactualización, enlaces, versionado, formatos de entrega.
- `references/plantillas.md` — README, ADR, runbook, ficha de módulo, ficha de pantalla.

## Skills hermanas (opcionales)

- [`skill-qa`](https://github.com/nanoizax/skill-qa) — pruebas funcionales y de regresión. Reutilízala para recorrer y verificar el sistema.
- [`skill-arquitectura-backend`](https://github.com/nanoizax/skill-arquitectura-backend) — criterios de diseño del backend; enlázala desde la documentación de arquitectura.
- Si tienes una guía de base de datos propia, úsala para el diccionario de datos. Nunca escribas en la base para documentar: consulta con un rol de solo lectura.
