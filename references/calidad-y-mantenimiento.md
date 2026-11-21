# Calidad y mantenimiento

## Checklist antes de entregar

**Verdad**
- [ ] Cada comando, URL, ruta y nombre de botón del doc existe y se ejecutó (registro en `VERIFICATION.md`).
- [ ] Lo no probado está marcado `Sin verificar` en el texto y en el registro; nada inventado.
- [ ] Sin afirmaciones de estado ("pendiente", "aprobado", "desplegado") sin evidencia fechada.
- [ ] Comandos tomados de referencias externas sin confirmar llevan "verificar en el entorno".

**Estructura**
- [ ] README permite arrancar desde cero en ~15 min a alguien que no conoce el proyecto.
- [ ] Un tipo Diátaxis por página; cabecera común con `ultima_verificacion` y `verificado_contra`.
- [ ] `docs/index.md` enlaza todo y dice a quién va cada parte.
- [ ] Explicado el porqué de la estructura y de los archivos clave (con evidencia o marcado como hipótesis).

**Técnica**
- [ ] Diagramas renderizan, nombres idénticos al código, máximo 6-8 elementos.
- [ ] OpenAPI valida sin errores; cada endpoint con descripción, ejemplo real y errores.
- [ ] Diccionario de datos cubre el 100 % de las tablas; las sin descripción están marcadas.
- [ ] Variables de entorno: las del código y las documentadas coinciden en ambos sentidos.
- [ ] Móvil: artefacto instalado y abierto, parámetros de build completos.

**Manual**
- [ ] Cada tarea: objetivo, pasos, resultado esperado, captura, qué hacer si falla.
- [ ] Glosario, FAQ y solución de problemas presentes y con textos reales de la interfaz.
- [ ] Idioma y variedad del mercado del cliente (tuteo sin voseo para Chile, etc.).

**Seguridad y autoría (bloqueantes)**
- [ ] Búsqueda de secretos en `docs/` y capturas: claves, tokens, contraseñas, correos y teléfonos reales, IPs internas. 0 hallazgos.
- [ ] Capturas: solo datos demo, sin metadatos, sin barra de direcciones con sesión.
- [ ] Sin menciones de herramientas, datos internos ni coautoría que la política del proyecto prohíba en README, docs, metadatos de PDF/DOCX, nombres de archivo, comentarios y commits. Ejecuta antes de publicar (ajusta la lista de términos a la política del proyecto):
  ```bash
  git grep -inE '<términos-prohibidos-por-política>' -- docs README.md   # árbol
  git log --all --format='%an|%cn|%B' | grep -icE '<términos-prohibidos-por-política>|coautor'   # historial: debe dar 0
  ```
  Sustituye el marcador por los términos que la política del proyecto prohíbe (por ejemplo nombres de herramientas y trailers de coautoría); ambos comandos deben dar 0.
- [ ] Autor de commits y de los documentos = empresa o persona responsable, sin líneas de coautoría en los mensajes.

## Detección de documentación desactualizada

| Qué se desfasa | Cómo detectarlo |
|---|---|
| Enlaces rotos | Verificador de enlaces (lychee o markdown-link-check; **verificar en el entorno**) |
| Formato y estilo | markdownlint; Vale para terminología (opcional) |
| Contrato de API | OpenAPI generado frente al publicado (oasdiff); linter en cada PR |
| Esquema de BD | Regenerar diccionario y `git diff --exit-code` sobre `docs/reference/base-de-datos` |
| Variables de entorno | Script que compara las usadas en el código con la tabla |
| Pantallas y manual | Script de recorrido de las tareas: falla si un botón del texto no existe; comparación visual de pantallas clave |
| Archivos citados | Script que extrae rutas citadas en docs y comprueba que existen |
| Frescura | Alerta si `ultima_verificacion` supera 90 días |
| Cobertura | Lista de módulos, rutas y tablas del código frente a lo documentado: porcentaje |

- [ ] Regla de PR: si cambia `src/routes/**`, `migrations/**` o pantallas, el PR toca su doc (comprobación por rutas en CI, `CODEOWNERS` por carpeta de docs).
- [ ] Cadencia: revisión en cada release y trimestral; ADR nuevo por decisión relevante; `CHANGELOG` actualizado.

## Versionado

- La documentación se versiona en el mismo repo y se etiqueta con el release que describe.
- `CHANGELOG` (Keep a Changelog, **verificar formato en el entorno**) + SemVer.
- Manual con historial de versiones en su última página.
- Si conviven varias versiones en producción, una rama o carpeta de docs por versión mayor.

## Formatos de entrega

Regla: **una sola fuente (Markdown)**; el resto se genera. Nunca editar a mano el PDF o el DOCX.

| Formato | Cuándo | Herramienta | Notas |
|---|---|---|---|
| Markdown en repo | Siempre; fuente única | Git del proyecto | Mermaid renderiza nativo en GitHub y GitLab |
| Sitio estático | Equipo y usuarios, búsqueda, versiones | Docusaurus, MkDocs, VitePress, Starlight (**verificar estado de mantenimiento de cada uno**) | Desplegar desde CI |
| PDF | Entrega a cliente, manual imprimible | Pandoc, o imprimir a PDF desde el sitio con navegador headless; o la skill `pdf` del sistema (**verificar disponibilidad**) | Portada, índice, versión, fecha, numeración, enlaces internos |
| DOCX | Cliente que edita o aprueba | Pandoc con plantilla de estilos, o la skill `docx` del sistema (**verificar disponibilidad**) | Imágenes a 150-200 dpi; revisar tablas y diagramas |
| Conversión de insumos | Cliente entrega PDF/DOCX/PPTX a leer | Skill `to-md` (**verificar disponibilidad**) | Convertir una vez a Markdown y trabajar sobre eso |
| Interfaz de API | Consumidores de API | Redoc, Scalar, Swagger UI | Generada de `openapi.yaml` |
| Sitio de BD | Consulta de esquema | tbls o SchemaSpy | Regenerar en CI |

Entregable de cliente (paquete mínimo): manual de usuario + guía de operación + resumen de arquitectura + `VERIFICATION.md`. Los accesos y credenciales se entregan por canal seguro aparte, **nunca dentro de los documentos**.

Al exportar PDF o DOCX:
- [ ] Portada con nombre del producto, versión, fecha y la empresa autora.
- [ ] Propiedades del archivo (autor, título, programa creador) revisadas: sin nombres de herramientas ni datos internos.
- [ ] Diagramas y capturas legibles en papel; tablas sin cortes.
- [ ] Abre el archivo final y revisa páginas al azar.

## Informe final (obligatorio)

1. Qué se documentó (lista de archivos y capas).
2. Qué se verificó y cómo (enlace a `VERIFICATION.md`).
3. Qué quedó **sin verificar** y por qué (sin emulador iOS, sin acceso a un servicio, etc.).
4. Hallazgos del estudio: riesgos, deuda técnica, secretos expuestos (reportar al responsable, sin copiarlos).
5. Deuda de documentación: qué falta y quién debería aportarlo.
6. Cómo mantenerla al día (comando de regeneración, CI, propietario).
